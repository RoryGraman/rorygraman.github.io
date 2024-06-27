---
order: 700
---

# GET /sources[chainId]

`/sources/[chainId]` requests can be called from any Quicknode endpoint enabled to use the API, regardless which chain/endpoint you have configured with the Meta-Aggregator Swap API. No json parameters are needed for this call.

```
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/sources/[chainId]
```

Knowing which sources are available for each chain not only shows you all the available DEX's for all the available Aggregators we support, it also allows you filter, or optionally only include sources for both `/quote` and `/swap` requests via `excludeSources`, or `includeSources`

## Usage

</br>
</br>

### `includeSources`

When `includeSources` is used with `/quote`or`/swap`, it will <b>only</b> return responses for that particular protocol. For example, if you set `includeSources: "uniswap-v3"` in a `/quote` or `/swap` request, the connected aggregators will only query Uniswap V3 pools for use in the swap.

</br>
</br>

### `excludeSources`

When `excludeSources` is used with `/quote`or`/swap`, it will return responses for every protocol <b>but</b> that pool type. For example, if you set `excludeSources: "uniswap-v3"` in a `/quote` or `/swap` request, it will filter out Uniswap V3 pools from the swap routes/calldata. Note that this doesn't eliminate Uniswap V2 forks, such as PancakeSwap V3, Sushiswap V3, or so on. Those pools will each have to be excluded as well.

## Example:

### Request

```json
curl -X GET "https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/sources/56"
```

### Response

```json
{
  "chainId": "56",
  "sources": "0x,1inch,aave,aave-v1,aave-v2,aave-v3,ambient,angle,
  angle-savings,angle-transmuter,ankr,apeswap,augustus-rfq,balancer,
  balancer-v1,balancer-v2,balancer-v2-composable-stable,balancer-v2-stable,
  balancer-v2-weighted,balancer-v2-wrapper,bamboodefi,bancor,bancor-carbon,
  bancor-v2,bancor-v3,bdai,beefy-finance,bgd-aave-static,blackholeswap,
  blueprint,blueprint-v3,bprotocol,button-wrappers,capital-dex,carbon,chai,
  chickenswap,claystack,clipper-rfq,clober,compound,compound-v2,compound-v3,
  convergence,convergence-x,convex-crv-depositor,convex-fxs-depositor,
  crowdswap-v2,crypto-com,curve,curve-3crv,curve-crypto-factory,
  curve-crypto-registry,curve-factory,curve-llamma,curve-registry,
  curve-stable-ng,curve-stable-plain,curve-tricrypto-ng,curve-twocrypto-ng,
  ..."
}
```

!!!If some mapped protocols are not showing up, such as new AMM pools or DEX's, drop us a line on [Telegram](https://t.me/rorygraman) and we'll get it added 😄
!!!
