---
order: 700
icon: terminal
---

# GET /aggregators

`/aggregators` requests can be called from any Quicknode endpoint enabled to use the API, and will automatically return a per-chain list of available aggregators based on the endpoint you are using in the URL

```
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/aggregators
or
https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/aggregators/[chainId]
```

!!!Optionally you can append a chainId to the `/aggregator` url in order to fetch the list of aggregators per chain, eg `/aggregators/1` or `/aggregators/56`. Note that when appending the chainId to the URL, it will override the default chainId of the endpoint you are using.
!!!

Knowing which aggregators are available for each chain not only shows you all the supported Aggregators for each chain we support, it also allows you filter, or optionally only include specific aggregators for both `/quote` and `/swap` requests via `excludeAggregators`, or `includeAggregators`

</br>
</br>

## Usage

</br>

### includeAggregators

When `includeAggregators` is used with `/quote`, it will <b>only</b> return responses for that particular protocol. For example, if you set `includeAggregators: "Paraswap"` in a `/quote` request, the API will only query Paraswap..

</br>
</br>

### excludeAggregators

When `excludeAggregators` is used with `/quote`, it will return responses for every aggregator other than what was specified. For example, if you set `excludeAggregators: "Parswap, OpenOcean"` in a `/quote` request, it will filter out Paraswap and OpenOcean from being queried.

!!!danger if you use `excludeAggregators` in conjunction with the `/swap` endpoint, make sure you're not excluding the Aggregator you specify in the `/swap/[aggregator]` endpoint, otherwise the API will fail to find any possible routes.
!!!

</br>
</br>

### Example:

Request

```json
curl -X GET "https://YOUR_QUICKNODE_ENDPOINT_HERE.com/addon/688/aggregators/56"
or
curl -X GET "https://YOUR_BSC_QUICKNODE_ENDPOINT_HERE.com/addon/688/aggregators"
```

Response

```json
{
  "chainId": "56",
  "aggregators": ["Paraswap", "Kyber", "Odos", "OpenOcean", "1inch", "0x"]
}
```

!!!If you'd like support for a particular Aggregator, drop us a line on [Telegram](https://t.me/rorygraman), or if are part of an Aggregator team that would like to be integrated, submit a PR in our [open-source](https://github.com/conveyorlabs/swap-api-aggregator-library) DEX aggregator library😄.
!!!
