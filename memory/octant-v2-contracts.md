# Octant v2 Contract Addresses

**Network:** Ethereum Mainnet
**Source:** Internal team (Feb 6, 2026)

## Mainnet Official (Production)

| Contract | Address | Block |
|----------|---------|-------|
| MorphoCompounderStrategyFactory | 0x052d20B0e0b141988bD32772C735085e45F357c1 | 23784110 |
| PaymentSplitterFactory | 0x5711765E0756B45224fc1FdA1B41ab344682bBcb | 23784110 |
| PaymentSplitter | 0x6D34ba4e49664F5C166d9B4f0B7F7c29bB69d3C0 | 23784110 |
| SkyCompounderStrategyFactory | 0xbe5352d0eCdB13D9f74c244B634FdD729480Bb6F | 23784110 |
| YearnV3StrategyFactory | 0x6D8c4E4A158083E30B53ba7df3cFB885fC096fF6 | 23784110 |
| YieldDonatingTokenizedStrategy | 0xb27064A2C51b8C5b39A5Bb911AD34DB039C3aB9c | 23784110 |

## Mainnet Test

### Vaults
| Contract | Address | Block |
|----------|---------|-------|
| LidoStrategyFactory | 0xA8C4c7Dd542957DaFfA3e3159F265Dd74Bc4B6B0 | 23677569 |
| MorphoCompounderStrategyFactory | 0xbb04FD82241d83720c7Ae02DD58678C1A9396162 | 23677569 |
| PaymentSplitterFactory | 0x669E92AdD5bDB0e0de72dffD5e40d637811B5ee4 | 23677569 |
| PaymentSplitter | 0xbb9D88a06e143FD20c7515d67236756E6e395B22 | 23677569 |
| RocketPoolStrategyFactory | 0x9bEE42006Bddf22F9c06B649559E4291DCFAf86B | 23677569 |
| SkyCompounderStrategyFactory | 0x0100BaC61f3D8388a74F53808FEc94161DDbBCF7 | 23677569 |
| YearnV3StrategyFactory | 0xA5e419182aD4B6C722214cEB241042F3B0284380 | 24234557 |
| YieldDonatingTokenizedStrategy | 0x5A9ae5CD2A7f48C7c047ae24e2679B33796A172f | 23677569 |
| YieldSkimmingTokenizedStrategy | 0xC7C8D2ae1896e3EbD56081a2EfbBB27df89701d9 | 23677569 |
| YieldSkimmingTokenizedStrategy | 0x94fB5d6A39C89499349F880A375b778Da12745Aa | 24234557 |
| LidoStrategyFactory | 0xc1C0d3C020074A0d0E102cc7EC8d26e8F10766aF | 24234557 |
| RocketPoolStrategyFactory | 0x4Ef3A121AAb13102A2a805EEc52C55E2819D74c1 | 24234557 |

### Regen Staker
| Contract | Address | Block |
|----------|---------|-------|
| AddressSetFactory | 0xB341D79A743B359EaCA67a2bDdABb325Bb327C77 | 24233967 |
| RegenStakerFactory | 0x3F5E928EAaCf4c578aB90Cf5344ef7498e8d72Fa | 24234020 |
| AddressSet: Staker allowset | 0x0dc3284de0235442837CD9126c7ba3AeDb6d6711 | 24234254 |
| AddressSet: Staker blockset | 0x1D49FebADcA80e4e5a5377Fe5F42cB7752d3b401 | 24234254 |
| AddressSet: Allocation mechanism allowset | 0x58b6e702D6EB040848E5De9ac488D18325Ce4627 | 24234254 |
| RegenEarningPowerCalculator | 0x3D607801B25C4fb761932cA04649282Ad64D4B56 | 24234455 |
| RegenStaker with delegation (SHU) | 0x3B2a97f07f2Cce570f8086E74Cdb37b8faC84382 | 24234505 |
| RegenStaker w/o delegation (GLM) | 0x770Ce153DCf81A7FF65E9F655fC42a7b8E9E9285 | 24234554 |

## Develop Config (JSON)

```json
{
  "mainnet": {
    "SkyCompounderStrategyFactory": {
      "address": "0xbe5352d0ecdb13d9f74c244b634fdd729480bb6f",
      "startBlock": 23773404
    },
    "MorphoCompounderStrategyFactory": {
      "address": "0x052d20b0e0b141988bd32772c735085e45f357c1",
      "startBlock": 23773404
    },
    "LidoStrategyFactory": {
      "address": "0xc1C0d3C020074A0d0E102cc7EC8d26e8F10766aF",
      "startBlock": 23773639
    },
    "YearnV3StrategyFactory": {
      "address": "0xA5e419182aD4B6C722214cEB241042F3B0284380",
      "startBlock": 23773639
    },
    "PaymentSplitterFactory": {
      "address": "0x5711765e0756b45224fc1fda1b41ab344682bbcb",
      "startBlock": 23773404
    }
  }
}
```

## Staging

### Vaults
| Contract | Address |
|----------|---------|
| MorphoCompounderStrategyFactory | 0xc91B651f770ed996a223a16dA9CCD6f7Df56C987 |
| PaymentSplitterFactory | 0xB90AcF57C3BFE8e0E8215defc282B5F48b3edC74 |
| SkyCompounderStrategyFactory | 0x934A389CaBFB84cdB3f0260B2a4FD575b8B345A3 |

### Regen Staker
| Contract | Address |
|----------|---------|
| RegenStaker | 0x9e6d52c8d1f7225413a4e3eedf40fa66daf4716e |

---

## Key Contracts for Dune Dashboard

**Primary (Production):**
- PaymentSplitter: `0x6D34ba4e49664F5C166d9B4f0B7F7c29bB69d3C0` (yield distribution)
- PaymentSplitterFactory: `0x5711765E0756B45224fc1FdA1B41ab344682bBcb`
- YieldDonatingTokenizedStrategy: `0xb27064A2C51b8C5b39A5Bb911AD34DB039C3aB9c`

**Strategy Factories:**
- MorphoCompounderStrategyFactory: `0x052d20B0e0b141988bD32772C735085e45F357c1`
- SkyCompounderStrategyFactory: `0xbe5352d0eCdB13D9f74c244B634FdD729480Bb6F`
- YearnV3StrategyFactory: `0x6D8c4E4A158083E30B53ba7df3cFB885fC096fF6`

**Start Block:** 23784110 (for historical queries)

---

## Frontend Environment Variables (Develop)

*Source: Quentin, Feb 11 2026*

```
VITE_API_URL=https://develop-backend.ov2sm.octant.build/
VITE_CPS_URL=https://cps.mainnet.octant.app/
VITE_MAINNET_RPC_URL=https://virtual-mainnet.rpc.octant.build
VITE_REGEN_STAKER_CONTRACT_ADDRESS=0xd42768953ad9f50b5d5dcebf8cbad41acd87c1e9
VITE_REOWN_PROJECT_ID=e18d98ecc189e9237c23d83cecfd3e7e
VITE_SENTRY_DSN=https://1f18c2b4c31219858819892634873961@o4506778713194496.ingest.us.sentry.io/4510118991626240
VITE_SENTRY_ENVIRONMENT=develop
VITE_SENTRY_TRACES_SAMPLE_RATE=1.0
VITE_SUBGRAPH_REGEN_URL=https://graph.ov2sm.octant.build/subgraphs/name/develop/octant-v2-subgraph-regen
VITE_OCTANT_V1_IPFS_GATEWAY=https://turquoise-accused-gayal-88.mypinata.cloud/ipfs
```

**GitOps repo (env configs):** https://github.com/golemfoundation/octant-v2-gitops/tree/master/charts/regen
**Note:** "None are secrets since it's frontend" (Quentin)
