# Ventuals Bug Bounty - Full Code Analysis

## Bounty Details
- **URL:** cantina.xyz/bounties/d1c3592e-f335-4235-94ac-c6bf6cdb50a3
- **Max Reward:** Up to $1M (Critical), $20K (High), $2K (Medium)
- **Cap:** 5% of direct funds at risk
- **Protocol:** vHYPE (HYPE liquid staking token on Hyperliquid)
- **GitHub:** github.com/ventuals/ventuals-contracts
- **Docs:** github.com/ventuals/ventuals-contracts/blob/main/docs/ARCHITECTURE.md

## Architecture Overview
Ventuals creates vHYPE to raise the 500K HYPE minimum stake for HIP-3 mainnet deployment.
- Deposit HYPE -> get vHYPE (ERC20, transferable)
- vHYPE/HYPE exchange rate increases over time as staking yield accrues
- Withdraw: queue -> batch process (1/day) -> finalize -> claim (7 days + buffer)
- All contracts are UUPS upgradeable proxies

## Contract Inventory (ALL IN SCOPE)

### 1. StakingVaultManager.sol (~600 LOC) - THE MAIN TARGET
- Handles deposits, withdrawals, minting/burning vHYPE
- Exchange rate calculation
- Batch processing of withdrawals
- Enforces minimum stake balance (500K HYPE)

### 2. StakingVault.sol (~200 LOC)
- Holds staked HYPE
- Wrapper around CoreWriter staking/delegation
- One-block delay enforcement for L1Read
- Validator whitelisting

### 3. VHYPE.sol (~40 LOC)
- Standard ERC20 (upgradeable)
- Only MANAGER can mint/burn
- Anyone can call burnFrom (via ERC20BurnableUpgradeable) BUT it's overridden to require onlyManager

### 4. RoleRegistry.sol (~80 LOC)
- Centralized RBAC (uses Solady's EnumerableRoles)
- 3 roles: OWNER, MANAGER, OPERATOR
- Pause/unpause per-contract
- Ownable2StepUpgradeable (2-step ownership transfer)

### 5. Base.sol (~60 LOC)
- Base contract for all upgradeable contracts
- Modifiers: whenNotPaused, onlyManager, onlyOperator, onlyOwner
- 50-slot storage gap

### 6. L1ReadLibrary.sol (~250 LOC)
- HyperCore precompile wrappers (0x800-0x810)
- Reads: positions, spot balances, delegations, delegation summaries, prices
- Key structs: SpotBalance, Delegation, DelegatorSummary

### 7. Converters.sol (not fetched, precision conversion library)
- Converts between 8-decimal (HyperCore) and 18-decimal (EVM)
- `to8Decimals()`, `to18Decimals()`, `stripUnsafePrecision()`

## Critical Code Paths

### Exchange Rate Calculation
```solidity
function exchangeRate() public view returns (uint256) {
    uint256 balance = totalBalance();
    uint256 totalSupply = vHYPE.totalSupply();
    if (totalSupply == 0) return 1e18;
    if (balance == 0) return 0;
    return Math.mulDiv(balance, 1e18, totalSupply);
}
```

### Total Balance (the tricky one)
```solidity
function totalBalance() public view returns (uint256) {
    uint256 accountBalances = stakingAccountBalance() + spotAccountBalance() + stakingVault.evmBalance();
    uint256 reservedHypeForWithdraws = totalHypeProcessed - totalHypeClaimed;
    require(accountBalances >= reservedHypeForWithdraws, ...);
    return accountBalances - reservedHypeForWithdraws;
}
```
- stakingAccountBalance = delegated + undelegated + totalPendingWithdrawal (from L1Read)
- spotAccountBalance = spot total balance (from L1Read)
- evmBalance = address(this).balance (native)

### Deposit Flow
1. User sends HYPE
2. `stripUnsafePrecision()` applied to msg.value
3. Mint vHYPE BEFORE transferring HYPE (important: rate based on pre-deposit balance)
4. Transfer HYPE to StakingVault

### Withdrawal Flow
1. `queueWithdraw`: escrow vHYPE, split into chunks if > maximumWithdrawAmount
2. `processBatch`: snapshot exchange rate, process queue until capacity reached or all done
3. `finalizeBatch`: burn escrowed vHYPE, transfer deposits to core, stake/unstake excess
4. `claimWithdraw`: after 7 days + claimWindowBuffer, transfer HYPE via spotSend

## Known Timing Issue (DOCUMENTED)
HyperEVM -> HyperCore transfers have one-block desync:
- EVM balance decreases immediately
- HyperCore spot balance (via L1Read) not updated until next block
- `totalBalance()` can be INCORRECT in the same block as a transfer
- They enforce `lastSpotBalanceChangeBlockNumber` checks

## Precision Handling
- HyperCore uses 8 decimals
- EVM uses 18 decimals
- Converters library handles conversion
- `stripUnsafePrecision()` removes sub-8-decimal precision from msg.value
- `to8Decimals()` and `to18Decimals()` for cross-layer conversion

## Potential Attack Surfaces (Initial Assessment)

### 1. Exchange Rate Manipulation
- Deposit mints vHYPE BEFORE transferring HYPE to vault (correct order)
- But what about the one-block delay? If totalBalance() reads stale L1Read data...
- Can an attacker deposit in the same block as a `transferHypeToCore` to get wrong rate?
- The `spotBalance` function has the one-block delay check, but `stakingAccountBalance` uses `delegatorSummary` which doesn't have the same check

### 2. Precision Loss (Balancer Pattern)
- Conversions between 8 and 18 decimals = potential rounding
- `to8Decimals()` rounds down (divides by 1e10)
- `stripUnsafePrecision()` strips last 10 digits
- Over many operations, could precision loss compound?
- Math.mulDiv used for exchange rate (good, prevents overflow)

### 3. Batch Processing Edge Cases
- What if processBatch is called with a cancelled withdrawal still in the queue?
- Cancelled withdrawals: `if (withdraw.cancelledAt == 0)` check exists in processBatch
- But the loop still advances `lastProcessedWithdrawId` even for cancelled ones
- Does this correctly handle the case where capacity is checked against a cancelled withdraw?

### 4. First Depositor Attack (Classic)
- If totalSupply == 0, exchange rate = 1e18 (1:1)
- First depositor gets vHYPE 1:1 with HYPE
- What if attacker deposits 1 wei, then donates massive HYPE to vault?
- Next depositor would get almost no vHYPE for their deposit
- Is there a minimum deposit check? YES: `minimumDepositAmount`
- But `canDeposit` modifier not shown in fetched code (truncated)

### 5. Withdrawal Queue Manipulation
- Linked list implementation: could there be ordering issues?
- `resetBatch` allows OWNER to undo processed withdrawals
- What happens if resetBatch is called while finalizeBatch is in progress?
- Can cancelled withdrawals cause gaps that affect batch capacity calculation?

### 6. One-Block Delay Bypass
- `spotBalance()` enforces `block.number > lastSpotBalanceChangeBlockNumber`
- But `delegatorSummary()` and `delegations()` don't have this check at the library level
- `_getDelegation` checks `lastDelegationChangeBlockNumber` per-validator
- Is there a way to get stale delegation data?

### 7. Slashing Logic
- `applySlash(batchIndex, slashedExchangeRate)` is owner-only
- What if the owner sets a wrong slashedExchangeRate?
- Users in affected batches get paid at slashed rate
- No validation that slashedExchangeRate < snapshotExchangeRate

### 8. vHYPE Burn / Donation Attack
- Anyone can approve MANAGER and MANAGER can burnFrom
- But burnFrom requires onlyManager, so external burn not possible
- What about the base ERC20BurnableUpgradeable? The burn() override requires onlyManager
- Safe: only MANAGER can burn

### 9. Decimal Conversion Precision
- `to8Decimals()` divides by 1e10 (rounds down)
- `to18Decimals()` multiplies by 1e10
- `spotSend` sends in 8 decimals: `hypeAmount.to8Decimals()`
- If hypeAmount has precision in the last 10 digits, it gets lost
- Could this lead to dust accumulation over many withdrawals?

### 10. finalizeBatch: Deposit/Withdrawal Netting
- `depositsInBatch = stakingVault.evmBalance()` (ALL EVM balance treated as deposits)
- What if someone sends HYPE directly to StakingVault (not via deposit)?
- This would inflate `depositsInBatch` and mess up the staking calculation
- But depositing HYPE would also inflate totalBalance() and thus the exchange rate
- Actually: StakingVault doesn't have a receive() function shown, so direct sends may work via fallback

## Priority Targets for Auditing
1. **Exchange rate manipulation via timing** (one-block delay)
2. **Precision loss in decimal conversions** (8 vs 18 decimal)
3. **Batch processing edge cases** (cancelled withdrawals, capacity checks)
4. **First depositor attack** (check minimumDepositAmount implementation)
5. **Direct HYPE transfers to vault** (bypassing deposit function)
6. **Slashing rate validation** (no bounds check)

## Cantina Submission Requirements
- Clear description of vulnerability and impact
- Steps to reproduce with proof of concept
- Conditions under which the issue occurs
- Potential implications if exploited
- Must be first to report (unique finding = valuable)
- Don't test on mainnet without permission
- Don't publicly disclose before written permission
