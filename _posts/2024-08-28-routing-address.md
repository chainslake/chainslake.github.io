---
title: Routing address - How to find the origin of wallets
layout: post
date: 2024-09-04 00:00
image: /assets/images/posts/2024-09-04/routing_address.png
headerImage: false
tag:
- ethereum
category: blog
author: lake
description: Routing address is an alternative to absolute address in the form of 0x... on Ethereum, built on deploy and fund activities, so that the origin and relationship between addresses can be easily determined.
---

Surely you are all familiar with the 0x address format on ethereum, it is created from a highly secure encryption algorithm, each address is random and unique so it is difficult to see the relationship between addresses in this format. By using the activity statistics of all addresses on Ethereum, Chainslake has built a conversion table between addresses in 0x format and a new format that we call Routing Address. With this new format, you can easily see the relationship between addresses that interact with each other, detect addresses with the same origin... You can see the image below or try the demo in [here](https://metabase.chainslake.io/public/dashboard/db6301d1-9043-43fe-9c3e-ad2729d9b28e){:target="_blank"}.

![Routing Address](/assets/images/posts/2024-09-04/routing_address.png)

## How is Routing Address generated?
To understand how a Routing Address is generated, you need to know about the Funder of an Address. Check any address on [etherscan.io](https://etherscan.io/) you will find Funder of an Address under More info.

![Funder of a wallet](/assets/images/posts/2024-09-04/funder_of_address.png){:style="width: 100%"}

![Deployer of a contract](/assets/images/posts/2024-09-04/deployer_of_address.png){:style="width: 100%"}

So the Funder of an Address is the first address that interacts with it since it was created or the address that deployed the contract. We built an address tree to represent the relationship between an address and its Funder and then indexed it chronologically to build the Routing Address.

## Types of Routing Addresses

Currently Chainslake provides 2 types of Routing Address including Native Routing Address and ERC20 Routing Address.
* *Native Routing Address*: Built on Transaction data, the space covers all addresses on Ethereum.
* *ERC20 Routing Address*: Built on ERC20 token transfer log data, the space only contains addresses that are holders of ERC20 tokens.

>  Note: Chainslake's Routing Address search function currently only supports Native Routing Address

## Applications of Routing Address

We have been building Insights using Routing Address with the following research directions:
* Identify and label addresses based on Routing Address.
* Clustering wallets, identifying wallets with the same origin, combining with other analysis...

## Conclusion

Any comments and requests for using our product, please send to the email address [lakechain.nguyen@gmail.com](mailto:lakechain.nguyen@gmail.com)