# Swap API Documentation

Welcome to the Swap API documentation! This guide provides detailed instructions on using our API for seamless token swaps. This documentation covers the `/quote`, `/swap`, `/sources`, and `/aggregators` endpoints, including their parameters, usage, and examples.

## Table of Contents

1. [Overview](#overview)
2. [Endpoints](#endpoints)
   - [/quote](#quote)
   - [/swap](#swap)
   - [/sources](#sources)
   - [/aggregators](#aggregators)
3. [Query Parameters](#query-parameters)
   - [Required Parameters](#required-parameters)
   - [Optional Parameters](#optional-parameters)
4. [Example Requests](#example-requests)
5. [Error Handling](#error-handling)
6. [Contact and Support](#contact-and-support)

## Overview

The Swap API provides a set of endpoints to interact with various decentralized exchanges (DEXs) and protocols, enabling seamless token swaps. This documentation focuses on the `/quote` and `/swap` endpoints, which provide quotes and execution details for token swaps, respectively.

## Endpoints

### /quote

**Endpoint:** `GET https://swap-api.xyz/quote`

**Description:** This endpoint provides a quote for a token swap, fetching the optimal route for the given parameters.

**Parameters:**

- `tokenIn` (required): The ERC20 token address of the token you want to sell.
- `tokenOut` (required): The ERC20 token address of the token you want to receive.
- `amountIn` (required): The amount of `tokenIn` tokens to swap (in base units).
- `slippage` (required): The maximum acceptable slippage percentage.
- `chainId` (required): The chain ID of the blockchain network.
- `recipient` (required): The address that will receive the swapped tokens.
- `includeProtocols` (optional): A comma-separated list of protocols to include.
- `excludeProtocols` (optional): A comma-separated list of protocols to exclude.
- `fromTokenDecimals` (optional): The decimal places for the `tokenIn` token.
- `toTokenDecimals` (optional): The decimal places for the `tokenOut` token.
- `includeAggregator` (optional): Include specific aggregators.
- `excludeAggregator` (optional): Exclude specific aggregators.

### /swap

**Endpoint:** `GET https://swap-api.xyz/swap/[aggregator]`

**Description:** This endpoint provides the necessary data to execute a token swap using a specific aggregator. This endpoint retrieves the optimal swap route, including details such as the recipient address, the amount of gas required, and the data needed to perform the swap transaction.

**Parameters:**

- `aggregator` (path, required): The name of the aggregator to use for the swap. Example values include `1inch`, `paraswap`, `odos`, etc.
- `tokenIn` (required): The address of the token to swap from.
- `tokenOut` (required): The address of the token to swap to.
- `slippage` (required): Acceptable slippage percentage. Example: `1.0` for 1%.
- `amountIn` (required): The amount of `tokenIn` to swap.
- `chainId` (required): The ID of the blockchain network. Example: `1` for Ethereum mainnet.
- `recipient` (required): The address that will receive the tokens.
- `includeProtocols` (optional): Comma-separated list of protocols to include in the swap.
- `excludeProtocols` (optional): Comma-separated list of protocols to exclude from the swap.
- `fromTokenDecimals` (optional): Number of decimals for the input token.
- `toTokenDecimals` (optional): Number of decimals for the output token.

### /sources

**Endpoint:** `GET https://swap-api.xyz/sources/{chainId}`

**Description:** This endpoint retrieves a list of available sources for a given blockchain network.

**Parameters:**

- `chainId` (path, required): The ID of the blockchain network. Example: `1` for Ethereum mainnet.

### /aggregators

**Endpoint:** `GET https://swap-api.xyz/aggregators/{chainId}`

**Description:** This endpoint retrieves a list of available aggregators for a given blockchain network.

**Parameters:**

- `chainId` (path, required): The ID of the blockchain network. Example: `1` for Ethereum mainnet.

## Query Parameters

### Required Parameters

- `tokenIn` (required): The ERC20 token address of the token you want to sell.
- `tokenOut` (required): The ERC20 token address of the token you want to receive.
- `amountIn` (required): The amount of `tokenIn` tokens to swap (in base units).
- `slippage` (required): The maximum acceptable slippage percentage.
- `chainId` (required): The chain ID of the blockchain network.
- `recipient` (required): The address that will receive the swapped tokens.

### Optional Parameters

- `includeProtocols`: A comma-separated list of protocols to include.
- `excludeProtocols`: A comma-separated list of protocols to exclude.
- `fromTokenDecimals`: The decimal places for the `tokenIn` token.
- `toTokenDecimals`: The decimal places for the `tokenOut` token.
- `includeAggregator`: Include specific aggregators.
- `excludeAggregator`: Exclude specific aggregators.

## Example Requests

### Example 1: Basic Request

**Request:**

```sh
curl -X GET "https://swap-api.xyz/quote?tokenIn=0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE&tokenOut=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&amountIn=1000000000000000000&slippage=1&chainId=1&recipient=0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5"
```

**Response:**

```json
{
  "best": {
    "aggregator": "1inch",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37458266850",
    "gas": "210000"
  },
  "1inch": {
    "aggregator": "1inch",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37458266850",
    "gas": "210000"
  },
  "Paraswap": {
    "aggregator": "Paraswap",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37300000000",
    "gas": "220000"
  }
}
```

### Example 2: Including Specific Protocols

**Request:**

```sh
curl -X GET "https://swap-api.xyz/quote?tokenIn=0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE&tokenOut=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&amountIn=1000000000000000000&slippage=1&chainId=1&recipient=0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5&includeProtocols=uniswap-v2,sushiswap"
```

**Response:**

```json
{
  "best": {
    "aggregator": "1inch",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37458266850",
    "gas": "210000"
  },
  "1inch": {
    "aggregator": "1inch",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37458266850",
    "gas": "210000"
  }
}
```

### Example 3: Excluding Specific Protocols

**Request:**

```sh
curl -X GET "https://swap-api.xyz/quote?tokenIn=0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE&tokenOut=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&amountIn=1000000000000000000&slippage=1&chainId=1&recipient=0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5&excludeProtocols=uniswap-v3"
```

**Response:**

```json
{
  "best": {
    "aggregator": "Paraswap",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37300000000",
    "gas": "220000"
  },
  "Paraswap": {
    "aggregator": "Paraswap",
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "amountIn": "1000000000000000000",
    "amountOut": "37300000000",
    "gas": "220000"
  }
}
```

## Error Handling

The Swap API uses standard HTTP status codes to indicate the success or failure of an API request. Here are the most common status codes you may encounter:

- **200 OK**: The request was successful.
- **400 Bad Request**: The request was invalid. Check the error message for details.
- **401 Unauthorized**: Authentication failed. Verify your API key.
- **500 Internal Server Error**: An error occurred on the server. Try again later.

### Example Error Response

```json
{
  "error": "Missing required query parameters: tokenIn, tokenOut, amountIn"
}
```

## Example Requests

### Example 1: Basic Swap Request

**Request:**

```sh
curl -X GET "https://swap-api.xyz/swap/odos?tokenIn=0x0000000000000000000000000000000000000000&tokenOut=0xa0b86991c6218b36c1d19D4a2e9eb0ce3606eb48&amountIn=100000000000000000&slippage=1.0&chainId=1&recipient=0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5"
```

**Response:**

```json
{
  "aggregator": "odos",
  "from": "0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5",
  "to": "0xCf5540fFFCdC3d510B18bFcA6d2b9987b0772559",
  "value": "100000000000000000",
  "data": "0x3b635ce400000000000000000000000...",
  "gas": 130918,
  "chainId": "1"
}
```

### Example 2: Swap Request with Included Protocols

**Request:**

```sh
curl -X GET "https://swap-api.xyz/swap/odos?tokenIn=0x0000000000000000000000000000000000000000&tokenOut=0xa0b86991c6218b36c1d19D4a2e9eb0ce3606eb48&amountIn=100000000000000000&slippage=1.0&chainId=1&recipient=0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5&includeProtocols=uniswap-v2,sushiswap"
```

**Response:**

```json
{
  "aggregator": "odos",
  "from": "0x92977D2552f455Bb9A3457AEbfCb78f1256Dd2e5",
  "to": "0xCf5540fFFCdC3d510B18bFcA6d2b9987b0772559",
  "value": "100000000000000000",
  "data": "0x3b635ce400000000000000000000000..."
}
```

## /sources

### Example Request

**Request:**

```sh
curl -X GET "https://swap-api.xyz/sources/1"
```

**Response:**

```json
{
  "chainId": "1",
  "sources": [
    "aave", "aave-v1", "aave-v2", "aave-v3", "ambient", "angle", "angle-savings", "angle-transmuter", "apeswap", "augustus-rfq", "balancer", "balancer-v1", "balancer-v2", "balancer-v2-composable-stable", "balancer-v2-stable", "balancer-v2-weighted", "balancer-v2-wrapper", "bamboodefi", "bancor", "bancor-v2", "bancor-v3", "bdai", "beefy-finance", "bgd-aave-static", "blackholeswap", "blueprint", "blueprint-v3", "bprotocol", "button-wrappers", "zeusswap", "zk-bob", ...
  ]
}
```

## /aggregators

### Example Request

**Request:**

```sh
curl -X GET "https://swap-api.xyz/aggregators/1"
```

**Response:**

```json
{
  "chainId": "1",
  "aggregators": ["Paraswap", "Kyber", "1inch", "0x", "Odos"]
}
```

## Contact and Support

For any issues, questions, or support, please reach out to our support team on [https://t.me/ConveyorLabs](Telegram).
