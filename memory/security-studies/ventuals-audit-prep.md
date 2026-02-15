# Ventuals Audit Prep: Attack Vectors + Submission Guide

## Attack Vector #1: HyperEVM/HyperCore Timing Desync (HIGHEST PRIORITY)

### Background (from Ambit Labs + Hyperliquid Docs)
- Core blocks: ~12/sec. EVM blocks: small (1/sec, 2M gas) + big (60/sec, 30M gas)
- EVM blocks execute INSIDE Core blocks, after Core transactions complete
- **L1Read precompiles always read from the CURRENT Core state** (start of the block)
- CoreWriter actions are QUEUED during EVM block, processed AFTER EVM block finalizes
- **EVM -> Core transfers happen in the SAME L1 block, immediately AFTER EVM block is built**
- **Core -> EVM transfers are queued until the NEXT EVM block**

### The Critical Sequence (per official docs):
1. L1 block is built
2. EVM block is built
3. EVM -> Core transfers are processed
4. CoreWriter actions are processed

### How Ventuals Handles This:
- `spotBalance()` enforces `block.number > lastSpotBalanceChangeBlockNumber` (one-block delay)
- `_getDelegation()` enforces `block.number > lastDelegationChangeBlockNumber[validator]`
- `transferHypeToCore()` sets `lastSpotBalanceChangeBlockNumber = block.number`
- `stake()` and `unstake()` set both `lastSpotBalanceChangeBlockNumber` and `lastDelegationChangeBlockNumber`

### Potential Attack: `delegatorSummary()` has NO per-block delay check
- `stakingAccountBalance()` calls `stakingVault.delegatorSummary()` which calls `L1ReadLibrary.delegatorSummary(address(this))`
- The L1ReadLibrary function has NO block-delay check. It just calls the precompile directly.
- BUT the StakingVault's `_getDelegation()` DOES check. These are different functions.
- `delegatorSummary()` (the external function) has NO block number check at all
- This means `totalBalance()` could use stale `delegatorSummary` data right after a `stake()` or `unstake()`
- **However:** `stake()` and `unstake()` also set `lastSpotBalanceChangeBlockNumber`, so `spotBalance()` would revert
- The question is: does `totalBalance()` ever get called in a path where `spotBalance` would revert but `delegatorSummary` is still accessible?
- ANSWER: `totalBalance()` calls both `stakingAccountBalance()` (uses delegatorSummary) AND `spotAccountBalance()` (uses spotBalance with check). So if `spotBalance` reverts, `totalBalance()` reverts too.
- **VERDICT: Likely protected, but edge cases around the `evmBalance()` path need more analysis**

### Potential Attack: Deposit in same block as `finalizeBatch`
- `finalizeBatch()` calls `transferHypeToCore()` which sets `lastSpotBalanceChangeBlockNumber`
- If someone calls `deposit()` in the same block AFTER `finalizeBatch()`, the deposit goes through
- `deposit()` calls `HYPETovHYPE(amountToDeposit)` which calls `exchangeRate()` which calls `totalBalance()`
- `totalBalance()` calls `spotAccountBalance()` which would REVERT because `lastSpotBalanceChangeBlockNumber == block.number`
- **VERDICT: Protected. Cannot deposit in same block as finalizeBatch.**

---

## Attack Vector #2: First Depositor / Inflation Attack

### Classic ERC-4626 Pattern
1. Attacker deposits 1 wei, gets 1 share
2. Attacker donates massive amount to vault (direct transfer)
3. Next depositor's shares = (their deposit * totalSupply) / totalAssets, rounds to 0 or 1
4. Attacker redeems their 1 share for half the pool

### Ventuals Specific Analysis
- `exchangeRate()`: if totalSupply == 0, returns 1e18 (1:1)
- First depositor gets `Math.mulDiv(amountToDeposit, 1e18, 1e18)` = amountToDeposit shares (1:1)
- **No "dead shares" or virtual offset protection**
- `minimumDepositAmount` exists but unclear what value it's set to
- **Can HYPE be sent directly to StakingVault?**
  - StakingVault has no `receive()` or `fallback()` shown
  - But `deposit()` is `payable` and HYPE is native gas token
  - Direct transfers to the vault address would increase `evmBalance()`
  - This would inflate `totalBalance()` and thus `exchangeRate()`

### Attack Scenario
1. Attacker is first depositor, deposits `minimumDepositAmount` (e.g. 1 HYPE)
2. Gets vHYPE 1:1
3. Sends large amount of HYPE directly to StakingVault address (via regular transfer)
4. `evmBalance()` increases, `totalBalance()` increases, `exchangeRate()` inflates
5. Next depositor gets far fewer vHYPE per HYPE deposited
6. Attacker withdraws, gets back more than they deposited

### BUT: Can you send HYPE directly to StakingVault?
- If StakingVault doesn't have `receive()` or `fallback()`, direct ETH/HYPE transfers would revert
- Need to check if there's any way to force-send HYPE (selfdestruct, coinbase rewards)
- On Hyperliquid, `selfdestruct` may not exist or work the same way
- **This needs testing on testnet**

**VERDICT: Medium risk. Depends on whether direct HYPE can be sent to vault.**

---

## Attack Vector #3: Decimal Precision Loss (8 vs 18)

### The Conversion Chain
- HyperCore uses 8 decimals (uint64)
- HyperEVM uses 18 decimals
- `to8Decimals()` divides by 1e10 (rounds DOWN)
- `to18Decimals()` multiplies by 1e10
- `stripUnsafePrecision()` zeroes out last 10 digits of 18-decimal value

### Where Precision Loss Occurs
1. `claimWithdraw`: `hypeAmount.to8Decimals()` sent via `spotSend`
   - If hypeAmount = 1_000_000_000_001 (18 dec), to8Decimals = 100 (8 dec)
   - User loses 1 wei worth of precision per claim
2. `stake()`: `amountToStake.to8Decimals()` 
3. `unstake()`: `amountToUnstake.to8Decimals()`
4. `deposit()`: `msg.value.stripUnsafePrecision()` (strips sub-8-dec precision at entry)

### Can This Be Exploited?
- Each individual loss is tiny (<1e-10 HYPE per operation)
- BUT: over thousands of withdrawals, dust accumulates in the vault
- This dust is unaccounted for, effectively donated to remaining vHYPE holders
- **Not exploitable for profit** (you can't extract the dust) but creates minor accounting drift
- **Unlikely to qualify as Medium severity** per Cantina standards

**VERDICT: Low/Informational. Precision loss exists but not economically exploitable.**

---

## Attack Vector #4: Batch Processing + Cancelled Withdrawals

### The Code Path
```
processBatch():
  while (withdrawCapacityAvailable > 0 && numWithdrawalsLeft > 0):
    get next withdraw from queue
    if (expectedHypeAmount > withdrawCapacityAvailable): break
    withdrawCapacityAvailable -= expectedHypeAmount
    if (withdraw.cancelledAt == 0):  // <-- only count non-cancelled
      batch.vhypeProcessed += withdraw.vhypeAmount
    withdraw.batchIndex = currentBatchIndex  // assigned even if cancelled!
    lastProcessedWithdrawId = nextNodeId
```

### The Issue
- A cancelled withdrawal STILL gets assigned to the batch (batchIndex set)
- Its HYPE amount is STILL subtracted from `withdrawCapacityAvailable`
- But it's NOT counted in `batch.vhypeProcessed` (because of the cancelledAt check)
- This means: cancelled withdrawals EAT capacity without being counted

### Attack Scenario
1. User queues many small withdrawals
2. User cancels some of them (but they're already in the queue)
3. When `processBatch` runs, cancelled items consume capacity
4. Fewer legitimate withdrawals get processed per batch
5. This is a **griefing attack** (not direct fund theft)

### How Severe?
- Cancel removes from linked list (`withdrawQueue.remove(withdrawId)`)
- Wait... if `cancelWithdraw` calls `withdrawQueue.remove(withdrawId)`, then the cancelled withdrawal is no longer in the queue
- So `processBatch` would never encounter it in the while loop
- **VERDICT: SAFE. Cancelled withdrawals are removed from the linked list before batch processing.**

---

## Attack Vector #5: `applySlash` Has No Bounds Validation

### The Code
```solidity
function applySlash(uint256 batchIndex, uint256 slashedExchangeRate) external onlyOwner {
    // No validation that slashedExchangeRate < snapshotExchangeRate
    // No validation that slashedExchangeRate > 0
    // No validation that batchIndex is within range
}
```

### Impact
- Owner could set `slashedExchangeRate = 0`, making all withdrawals in that batch worthless
- Owner could set `slashedExchangeRate > snapshotExchangeRate`, giving users MORE than expected
- This is an admin/trust issue, not a smart contract bug per se

**VERDICT: Informational. Admin function without input validation. Depends on whether Cantina considers centralization risks in scope.**

---

## Submission Format (Best Practices)

### Structure (from JohnnyTime + Code4rena examples)
1. **Title:** Clear, specific, one sentence
2. **Lines of code:** Link to exact lines
3. **Vulnerability details:** What's wrong and why
4. **Impact:** What can happen (fund loss, griefing, etc.)
5. **Proof of Concept:** Step-by-step attack scenario, ideally with code
6. **Recommended Mitigation:** How to fix it

### Quality Tips
- SHORT and CONCISE. Judges have limited time.
- Focus on IMPACT, not just the bug description
- Include PoC whenever possible (Foundry tests ideal)
- Don't submit low-quality findings hoping to get lucky
- Unique findings worth exponentially more than duplicates
- Specific code references with line numbers
- Clear attack scenario with numbered steps

### Severity Guide (Cantina)
| Impact/Likelihood | High | Medium | Low |
|---|---|---|---|
| High | Critical | High | Medium |
| Medium | High | Medium | Low |
| Low | Medium | Low | Informational |
