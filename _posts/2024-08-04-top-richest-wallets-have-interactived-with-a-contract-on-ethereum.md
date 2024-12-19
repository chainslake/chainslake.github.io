---
title: Top richest wallets have interactived with a contract on Ethereum
layout: post
date: 2024-08-04 09:00
image: /assets/images/posts/2024-08-04/query_top_richest_wallet.png
headerImage: false
published: false
tag:
- ethereum
category: blog
author: lake
description: Explain how to build the API on chainslake which get top richest wallets have interactived with one contract on ethereum
---

Hello guys, my name is Lake, founder of Chainslake - A Blockchain data lakehouse framework. I just solved the problem of getting a list of the richest wallets have interacted with any contract on Ethereum, so I wrote this article to share with you how I did it. You can try it out through the API or UI which I provided at the end of the article. Now let's get started.

## Why I need this knowledge?

Hmm, after deploying a smart contract to mainnet, I need to monitor the wallets that interact with my smart contract, especially the rich wallets, to plan the best service for them. I also need to know the rich wallets that interact with similar contracts like mine, so that I can send them gifts (Airdrop) to attract them to my product. 

## What I had

In the chainslake data warehouse, I have a `ethereum_balances.erc20_usd` table, this table contains the latest balance information of all wallets and all erc20 tokens on ethereum, along with the price in usd of each token.

![Balances ERC20 USD](/assets/images/posts/2024-08-04/balances.erc20_usd.png){:style="width: 100%"}

I also have a `ethereum.transactions` table, which will tell me which wallets have interacted with which addresses (smart contracts).

## What needs to be built further

Since the `ethereum.transactions` table is very large (it contains all transactions on ethereum), I created an intermediate table called `ethereum_activity.address_interactive` to summarize the number of interactions between the wallet and the contract day by day.

![Activity Address Interactive](/assets/images/posts/2024-08-04/activity_address_interactive.png)

## Write query to get insight

Using chainslake's playground metabase, I wrote a query to return the list of wallets I needed, with 3 parameters as input:

  - Contract address: [0x881D40237659C251811CEC9c364ef91dC08D300C](https://etherscan.io/address/0x881D40237659C251811CEC9c364ef91dC08D300C){:target="_blank"} (Metamask Swap router)
  - Block date: Previous 30 days
  - Token address: [0xdac17f958d2ee523a2206206994597c13d831ec7](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7){:target="_blank"} (Tether USD stable coin)

The results are the top 10 wallets holding the most USDT tokens that have interacted with the *Metamask Swap router* in the last 30 days:

![Query top richest wallets](/assets/images/posts/2024-08-04/query_top_richest_wallet.png)

## Try in UI

You can try it [here](https://metabase.chainslake.io/question/171-top-richest-wallets-have-interactived-with-a-contract){:target="_blank"}. Please email me if you don't have an account at chainslake or try in public dashboard [here](https://metabase.chainslake.io/public/dashboard/63a610b0-7957-4860-96fa-71a6c14dc78c){:target="_blank"}

## Try API 

Here are the details of the API for you to try, you will need an API_KEY to use it. Please email me to get your API_KEY or using test api key: `mb_DrYcueOZJcqDMTXBmljppUfs2Kyn04Nl2VlEErHV8hU=`. Have nice day!

*Request*
```sh
curl 'https://metabase.chainslake.io/api/card/171/query' \
  -H 'accept: application/json' \
  -H 'content-type: application/json' \
  -H 'X-API-KEY: API_KEY' \
  --data-raw '{"ignore_cache":true,"collection_preview":false,"parameters":[{"id":"0540fe31-dd88-460c-802e-681ded71ba44","type":"category","value":["0x881D40237659C251811CEC9c364ef91dC08D300C"],"target":["variable",["template-tag","contract_address"]]},{"id":"e154dd7b-1b00-4edf-9cdb-5aed4368e09e","type":"date/all-options","value":"past30days","target":["dimension",["template-tag","block_date"]]},{"id":"6240c089-f83b-47ad-adca-05bc8c49e863","type":"category","value":"0xdac17f958d2ee523a2206206994597c13d831ec7","target":["variable",["template-tag","token_address"]]}]}'
```

*Response*
```json
{"data":{"rows":[
["0x9ce002f1e53a047b6b56c80856d00a003e62c8d2",5189956.22360461],
["0x23f3fa943b5fe7a9300c8f43834a75bafb023d70",1510652.4205605995],
["0x3c922e98eb4ea36eb6b3bc3d78b0ebcbb60a7059",1229093.6540925985],
["0xf6a034476b277674b003abbeaa2ca3a483f31df0",900118.0651925262],
["0x670094c0d5b38feaaa4aa2ebed8471e4e9af9239",853714.809447288],
["0xacc4e86551153a395330aea9bdd4fd5060322bf3",814800.9128818855],
["0x7ea3f3458262c88187d1cd41913aa252ad1526e6",720303.6935034429],
["0x36d4ba437e3302c4c8dfa82aa636be49f33b6435",515570.41063297377],
["0x23ebddac426b8c4e6ae1b7d3734a99fe98ad2b03",504315.4080324716],
["0xd7f6577c78bdad7b76c6fa2f11853f77bc600733",488083.47738853167]
],
"cols":[{"display_name":"holder_address","source":"native","field_ref":["field","holder_address",{"base-type":"type/Text"}],"name":"holder_address","base_type":"type/Text","effective_type":"type/Text"},{"display_name":"total_balance","source":"native","field_ref":["field","total_balance",{"base-type":"type/Float"}],"name":"total_balance","base_type":"type/Float","effective_type":"type/Float"}]}}
```


