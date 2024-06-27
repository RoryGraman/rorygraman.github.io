---
order: 800
icon: terminal
---

# POST /swap

`/swap` requests should be called via the Quicknode endpoint you used to configure on each chain. Parameters are passed in via JSON.

```
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/swap
or
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/swap/[aggregator]
```

!!!Optionally you can append an aggregator name to the swap url in order to fetch calldata from an aggregator, eg `/swap/Paraswap` or `/swap/OpenOcean`. Note that when appending the aggregator name to the URL, it will override the `aggregator` parameter in the request body.
!!!

</br>
</br>

## Usage

</br>

### Required Parameters

| Parameter | Notes                                                     | Example                                      |
| --------- | --------------------------------------------------------- | -------------------------------------------- |
| tokenIn   | The ERC20 token address of the token you want to sell.    | "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE" |
| tokenOut  | The ERC20 token address of the token you want to receive. | "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48" |
| amountIn  | The amount of `tokenIn` tokens to swap (in base units).   | "1000000000000000000"                        |
| slippage  | The maximum acceptable slippage percentage.               | 1                                            |
| chainId   | The chain ID of the blockchain network.                   | 1                                            |
| recipient | The address that will receive the swapped tokens.         | "0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5" |

</br>
</br>

### Optional Parameters

| Parameter         | Notes                                                                                       | Example                      |
| ----------------- | ------------------------------------------------------------------------------------------- | ---------------------------- |
| aggregator        | Required if not used directly in URL. Having it in the URL overrides this value             | "Paraswap" or /swap/Paraswap |
| includeProtocols  | A comma-separated list of protocols to include.                                             | "uniswap-v2,sushiswap"       |
| excludeProtocols  | A comma-separated list of protocols to exclude.                                             | "uniswap-v3"                 |
| fromTokenDecimals | The decimal places for the `tokenIn` token. Consumes Quicknode RPC credits if not included  | 18                           |
| toTokenDecimals   | The decimal places for the `tokenOut` token. Consumes Quicknode RPC credits if not included | 6                            |
| zeroExApiKey      | A valid 0x API key is required in order to fetch from this source                           | "YOUR_0X_API_KEY"            |
| oneInchApiKey     | A valid 1inch API key is required in order to fetch from this source                        | "YOUR_1INCH_API_KEY"         |

!!!To request and receive responses from 0x and 1inch, you must pass in `zeroExApiKey` and/or `oneInchApiKey` parameters
!!!

</br>
</br>

### Premium (optional) Parameters

!!! These parameters are a premium feature of the swap-api. Purchase a premium plan in order to set your own fee
!!!

| Parameter             | Notes                                                                   | Example                                      |
| --------------------- | ----------------------------------------------------------------------- | -------------------------------------------- |
| odosReferralCode      | Referral code for Odos. Obtain a code [here](https://referral.odos.xyz) | "12345678"                                   |
| partnerReferralWallet | Wallet address for partner referral (premium only).                     | "0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5" |
| partnerReferralFeeBps | Fee in basis points for partner referral (premium only).                | 50                                           |

### Example:

Request

```json
curl -X POST "https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/swap"
-H "Content-Type: application/json"
-d '{
    "aggregator": "Paraswap",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "slippage": 1,
    "chainId": 1,
    "recipient": "0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5",
    "includeProtocols": "uniswap-v2,sushiswap",
    "excludeProtocols": "uniswap-v3",
    "fromTokenDecimals": 18,
    "toTokenDecimals": 6,
}'
```

Response

```json
{
  "from": "0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5",
  "to": "0xDEF171Fe48CF0115B1d80b88dc8eAB59176FEe57",
  "value": "1000000000000000000",
  "data": "0x0b86a4c1000000000000000000000000eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee0000000000000000000000
  000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000000000000000c670edf
  8000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20000000000000000000000000000000000000000
  0000000000000000000000a0000000000000000000000000000000000000000000000000000000000000000100000000000000000
  0004de5b4e16d0168e52d35cacd2c6185b44281ec28c9dc",
  "chainId": 1,
  "aggregator": "Paraswap"
}
```

</br>
</br>

Calling the `/swap` endpoint with your desired parameters automatically standardizes and passes your parameters to the desired DEX aggregator and returns a response based on values from their native API.

`to` is the aggregator router </br>
`from` is the wallet that the transaction is being signed from </br>
`value` is the base unit amount in Ether that is being transacted (specifically for Eth -> Token swaps)

!!!danger Make sure that the `from` address has approval to spend the `amountIn` for the `tokenIn` assets value for `Token -> Token` and `Token -> Eth` swaps otherwise the transaction will revert due to insufficient approval
!!!
