# Haust Devnet deployment

- Chain ID: `1008818835`
- RPC: `https://haust-devnet-rpc.eu-north-2.gateway.fm`
- Explorer: `https://haust-devnet-blockscout.eu-north-2.gateway.fm`

## Core

| Contract              | Address                                      |
| --------------------- | -------------------------------------------- |
| PoolAddressesProvider | `0x8538f34e5b1F435885150e13A81025545d1517A9` |
| Pool (proxy)          | `0x2B4B51939f1a427B3C9fb2B0A9Ce2bA68538AdFa` |
| PoolConfigurator      | `0x962A1397F85beA2a0dF3ECAf91aa10b1A8cBEd97` |
| ACLManager            | `0x8370Ca2Fba1A72aEF25DFbFE3731AED4b26887d3` |
| ACL admin             | `0x65a6E0F0A2CfAbE97Af540651B6990669403629C` |
| AaveOracle            | `0x027C9737F89e0a573E46Da906bCeb51AAD0b1344` |
| PoolDataProvider      | `0xf8a4eE238e19C7905D611f56d1f1054CfF01B0C4` |

## Periphery

| Contract              | Address                                      |
| --------------------- | -------------------------------------------- |
| WrappedTokenGatewayV3 | `0x409838473518b2C2f5f908749bDa3Fd8EBcB130A` |
| UiPoolDataProvider    | `0x557A80FEfb72675e65F544Dbd51fA7E3d7b37F0b` |
| WalletBalanceProvider | `0x100b00E632B4dBC70d455bc84CC779c69c2B8Cb3` |
| RewardsController     | `0x70Dd9c4E619244D2Cfd12b43E45bC28ec80710F6` |

## Reserves

| Symbol | Address                                      |
| ------ | -------------------------------------------- |
| USDT   | `0xb9882Bc4F209D6Bb26e971874779AA7447F82aa7` |
| USDC   | `0xad73118D8A179C17A2653E7342977e82E54cc4A7` |
| WBTC   | `0x75b69949D11013856e60B7386C28163daed1dE81` |
| WETH   | `0x1a1aF9C78704D3a0Ab9e031C92E7bd808711A582` |
| WHAUST | `0x2C990DAddaF3b760443b512Da9f001f721951438` |

WHAUST tokens: aToken `0xBFa52144a8D9A9C4A3fC005F3F20d45bA4fDd21e`, variable debt token `0x286e23CC5B3b00dE15f198248F80AF9481C00cd3`.

## WrappedTokenGatewayV3

Lets users supply, withdraw, repay and borrow native HAUST; the gateway wraps it into WHAUST and unwraps it back.

- Constructor args: `weth` = WHAUST, `owner` = ACL admin, `pool` = Pool proxy.
- Deployment tx: `0xabc523c97bd99629f696a8aac3be1c23b2bd9829a0281bf0cc5befdf7f167c46`
- Verified on Blockscout.

Approvals required from the user:

| Method                  | Approval                                                   |
| ----------------------- | ---------------------------------------------------------- |
| `depositETH`            | none                                                       |
| `repayETH`              | none                                                       |
| `withdrawETH`           | `approve` aWHAUST to the gateway                           |
| `withdrawETHWithPermit` | aWHAUST permit signature                                   |
| `borrowETH`             | `approveDelegation` on WHAUST variable debt to the gateway |

Redeploy:

```sh
forge create src/contracts/helpers/WrappedTokenGatewayV3.sol:WrappedTokenGatewayV3 \
  --rpc-url https://haust-devnet-rpc.eu-north-2.gateway.fm \
  --private-key $PRIVATE_KEY --broadcast \
  --constructor-args \
    0x2C990DAddaF3b760443b512Da9f001f721951438 \
    0x65a6E0F0A2CfAbE97Af540651B6990669403629C \
    0x2B4B51939f1a427B3C9fb2B0A9Ce2bA68538AdFa
```
