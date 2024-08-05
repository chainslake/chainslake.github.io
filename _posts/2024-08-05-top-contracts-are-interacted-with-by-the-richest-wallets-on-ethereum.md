---
title: Top contracts are interacted with by the richest wallets on Ethereum
layout: post
date: 2024-08-05 00:00
image: /assets/images/posts/2024-08-05/query_top_contract.png
headerImage: false
tag:
- ethereum
category: blog
author: lake
description: Explain how to build the API on chainslake which get contracts are interacted with by the richest wallets on Ethereum
---

Continuing from the previous post, today’s insight is the top contracts that receive the most interactions from the richest wallets on ethereum. The API for this insight is ready and you can find it at the end of this post.

## Why I need this knowledge?

The reason is quite simple, as an investor, I want to know which contracts are getting the most attention from rich wallets, those will be potential protocols and projects to consider investing in.

## What I had

I will use the 2 `ethereum_activity.address_interactive` and `ethereum_balances.erc20_usd` tables mentioned in the previous article, they are enough to find the insight we need in this situation.

- `ethereum_activity.address_interactive`
![Activity Address Interactive](/assets/images/posts/2024-08-04/activity_address_interactive.png)

- `ethereum_balances.erc20_usd`
![Balances ERC20 USD](/assets/images/posts/2024-08-04/balances.erc20_usd.png){:style="width: 100%"}

## Write query to get insight

I wrote a query with input of 2 parameters:

  - Block date: Previous 30 days
  - Token address: [0xdac17f958d2ee523a2206206994597c13d831ec7](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7){:target="_blank"} (Tether USD stable coin)

The results are the top 10 contract addresses that received interactions from the richest wallets, the total number of wallets that interacted, and the total assets (USDT) of the wallets that interacted with the contract.

![Query top contracts](/assets/images/posts/2024-08-05/query_top_contract.png)

In this query, I took all wallets and contracts that interacted with each other in the last 30 days, then joined with balance to calculate the total number of wallets interacting with each contract and the total balance of these wallets, and finally sorted and took the 10 contracts with the largest total balance.

You can try it [here](https://metabase.chainslake.io/question/172-top-contracts-are-interacted-with-by-the-richest-wallets){:target="_blank"}. Please email me if you don't have an account at chainslake.

## Try API

Here are the details of the API for you to try, you will need an API_KEY to use it. Please email me to get your API_KEY. Have nice day!

*Request*

```sh
curl 'https://metabase.chainslake.io/api/card/172/query' \
  -H 'accept: application/json' \
  -H 'content-type: application/json' \
  -H 'X-API-KEY: API_KEY' \
  --data-raw '{"ignore_cache":false,"collection_preview":false,"parameters":[{"id":"fe72eb56-d774-4472-a465-727e4722f6aa","type":"date/all-options","value":"past30days","target":["dimension",["template-tag","block_date"]]},{"id":"ecb92861-4133-4425-858b-0a2be7d73521","type":"category","value":"0xdac17f958d2ee523a2206206994597c13d831ec7","target":["variable",["template-tag","token_address"]]}]}'
```

*Response*

```json
{"data":{"rows":[
["0xdac17f958d2ee523a2206206994597c13d831ec7",1.6634237524051046E10,321529],
["0x5faa989af96af85384b8a938c2ede4a7378d9875",9.147309200834097E9,1236],
["0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",8.800004781138119E9,63462],
["0x6de037ef9ad2725eb40118bb1702ebb27e4aeb24",8.226243929869385E9,6379],
["0x7d1afa7b718fb893db30a3abc0cfc608aacfebb0",7.609509178425188E9,3196],
["0xc5f0f7b66764f6ec8c8dff7ba683102295e16409",7.343863378429938E9,82],
["0xbbbbca6a901c926f240b89eacb641d8aec7aeafd",7.215676013574053E9,367],
["0x514910771af9ca656af840dff83e8264ecf986ca",7.152806239996676E9,3773],
["0x3506424f91fd33084466f402d5d97f05f8e3b4af",6.974798881175304E9,490],
["0x0d8775f648430679a709e98d2b0cb6250d2887ef",6.966773126890833E9,392]
],
"cols":[{"display_name":"to","source":"native","field_ref":["field","to",{"base-type":"type/Text"}],"name":"to","base_type":"type/Text","effective_type":"type/Text"},{"display_name":"total_balance","source":"native","field_ref":["field","total_balance",{"base-type":"type/Float"}],"name":"total_balance","base_type":"type/Float","effective_type":"type/Float"},{"display_name":"total_wallets","source":"native","field_ref":["field","total_wallets",{"base-type":"type/BigInteger"}],"name":"total_wallets","base_type":"type/BigInteger","effective_type":"type/BigInteger"}]}}
```