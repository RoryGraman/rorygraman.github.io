---
order: 700
---

# GET /aggregators/[chainId]

`/aggregators/[chainId]` requests can be called from any Quicknode endpoint enabled to use the API, regardless which chain/endpoint you have configured with the Meta-Aggregator Swap API. No json parameters are needed for this call.

```
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/aggregators/[chainId]
```

Knowing which aggregators are available for each chain not only shows you all the supported Aggregators for each chain we support, it also allows you filter, or optionally only include specific aggregators for both `/quote` and `/swap` requests via `excludeAggregators`, or `includeAggregators`

## Usage

</br>
</br>

### `includeAggregators`

When `includeAggregators` is used with `/quote`, it will <b>only</b> return responses for that particular protocol. For example, if you set `includeAggregators: "Paraswap"` in a `/quote` request, the API will only query Paraswap..

</br>
</br>

### `excludeAggregators`

When `excludeAggregators` is used with `/quote`, it will return responses for every aggregator other than what was specified. For example, if you set `excludeAggregators: "Parswap, OpenOcean"` in a `/quote` request, it will filter out Paraswap and OpenOcean from being queried.

!!!danger if you use `excludeAggregators` in conjunction with the `/swap` endpoint, make sure you're not excluding the Aggregator you specify in the `/swap/[aggregator]` endpoint, otherwise the API will fail to find any possible routes.
!!!

## Example:

### Request

```json
curl -X GET "https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/aggregators/56"
```

### Response

```json
{
  "chainId": "56",
  "aggregators": ["Paraswap", "Kyber", "Odos", "OpenOcean", "1inch", "0x"]
}
```

!!!If you'd like support for a particular Aggregator, drop us a line on [Telegram](https://t.me/rorygraman), or if are part of an Aggregator team that would like to be integrated, submit a PR in our [open-source](https://github.com/conveyorlabs/swap-api-aggregator-library) DEX aggregator library😄.
!!!
