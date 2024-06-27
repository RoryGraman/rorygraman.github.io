---
order: 900
icon: terminal
---

# POST /quote

`/quote` requests should be called via the Quicknode endpoint you used to configure on each chain. Parameters are passed in via JSON.

```
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/quote
```

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

| Parameter         | Notes                                                                | Example                |
| ----------------- | -------------------------------------------------------------------- | ---------------------- |
| includeProtocols  | A comma-separated list of protocols to include.                      | "uniswap-v2,sushiswap" |
| excludeProtocols  | A comma-separated list of protocols to exclude.                      | "uniswap-v3"           |
| fromTokenDecimals | The decimal places for the `tokenIn` token.                          | 18                     |
| toTokenDecimals   | The decimal places for the `tokenOut` token.                         | 6                      |
| includeAggregator | Include specific aggregators.                                        | "1inch"                |
| excludeAggregator | Exclude specific aggregators.                                        | "paraswap"             |
| zeroExApiKey      | A valid 0x API key is required in order to fetch from this source    | "YOUR_0X_API_KEY"      |
| oneInchApiKey     | A valid 1inch API key is required in order to fetch from this source | "YOUR_1INCH_API_KEY"   |

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
curl -X POST "https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/quote"
-H "Content-Type: application/json"
-d '{
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
  "best": {
    "aggregator": "OpenOcean",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "3378798390",
    "gas": "350520",
    "amountOutMin": "3345010406"
  },
  "OpenOcean": {
    "aggregator": "OpenOcean",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "3378798390",
    "gas": "350520",
    "amountOutMin": "3345010406"
  },
  "Paraswap": {
    "aggregator": "Paraswap",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "3363203046",
    "gas": "111435",
    "amountOutMin": "3329571015"
  },
  "Odos": {
    "aggregator": "Odos",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "3363203046",
    "gas": "229928",
    "amountOutMin": "3329571015"
  },
  "Kyber": {
    "aggregator": "Kyber",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "3360473789",
    "gas": "185000",
    "amountOutMin": "3326869051"
  }
}
```

Calling the `/quote` endpoint with your desired parameters automatically standardizes and passes your parameters to several DEX aggregators and returns a response based on values from their native API. The `body.best` collection populates based on which aggregator provides the most favorable value on the trade. Additionally `body.best.aggregator` can then be used in the [/swap](../swap) endpoint to generate calldata

Additionally, you can filter out requests from specific aggregators via the `excludeAggregator` parameter, or specifically only include aggregators via `includeAggregators`
