---
title: Ethereum Data in Chainslake
layout: post
date: 2025-08-17 06:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- english
category: english
author: lake
description: Ethereum is one of the most important and popular blockchains today, many other blockchains are built on Ethereum's virtual machine technology, they are called EVM blockchains. This article will help you understand and exploit data on Ethereum, thereby easily expanding to other EVM blockchains.
---

In the [previous article](/query-data-in-chainslake/), you were introduced to organizing tables and how to query data effectively on the Chainslake platform. In this article, I will share about Ethereum data in Chainslake. Understanding Ethereum will help you exploit data from all other EVM blockchains.

# Contents
1. [Raw data on Ethereum](#raw_tables)
2. [Decode data, get information from contract](#decode_data)
3. [Build insight tables](#insight_data)
4. [Conclusion](#conclusion)

## Raw data on Ethereum<a name="raw_tables"></a>

Raw data on Ethereum includes 3 tables `transactions`, `logs`, `traces` located in the [ethereum](https://metabase.chainslake.com/browse/databases/3/schema/ethereum){:target="_blank"} directory.

#### Transactions

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjI4fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/transactions.png" alt="transactions" width="100%"></a></p>

This is a table containing information of all transactions on Ethereum, each transaction includes:

- block_date, block_number, block_time: Information about the time and block number of the transaction, these columns are all indexed.

- hash: Hash value is different for each transaction.

- index: Order of transactions in the block
- from: Address to send transactions
- to: Address to receive transactions, can be a normal wallet address or the address of a contract.
> Contract can be simply understood as a program running on the Ethereum virtual machine. The contract's execution code is sent to the blockchain through a transaction called Deploy. After Deploy, the contract can execute the functions (methods) it is programmed for if it is called by the user or other contracts. Each contract will have a unique address, the contract's logic cannot be deleted or changed after Deploy.
- gas_limit, gas_price, gas_used: are respectively the gas limit, gas price, gas used in the transaction
> On Ethereum, each transaction requires gas (fuel) purchased with ETH to execute.

- success: Whether the transaction is successful or not
> Note: If the transaction fails (for any reason such as error, lack of gas, ...) the pre-transaction state will be restored, but the sending address will still pay the gas fee for this transaction.

- value: The amount of ETH sent with the transaction (deducted directly from the sending address)

- method_id, data: Data (message) included with the transaction. If the destination is a contract, data is the input parameter when calling the functions in the contract.

#### Traces

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjI3fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/traces.png" alt="traces" width="100%"></a></p>

When a contract executes a transaction, it will include many sequential processing steps, these processing steps can call other functions in the same contract or other contracts, these are called internal transactions. The traces table stores information of all internal transactions on Ethereum. Including the following information:

- block_date, block_number, block_time: Information about the time and block number of the transaction, these columns are all indexed.
- hash: Hash value of the main transaction
- gas, gas_used: Gas provided and gas used in the internal transaction
- from, to: address of the contract sending and receiving
- input, output: Input data and return data
- method_id: id of the method called
- success: Is it successful?
> Note: If only 1 internal transaction fails, the entire transaction will fail

#### Logs

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjE5fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/logs.png" alt="logs" width="100%"></a></p>

During execution, a contract can emit events to record changes in the data inside the contract. Data from these events is stored in the logs table. Including the following information:

- block_date, block_number, block_time: Information about the time and block number of the transaction, these columns are all indexed.
- contract_address: the address of the contract that emitted the event
- tx_hash: hash of the main transaction
- topic1, topic2, topic3, topic4, data: data inside the event, all are encrypted and need to be decrypted if you want to use it (will be explained more clearly in the data decryption section).

> Note: The logs table only contains events of successful transactions

## Decode data, get information from contract<a name="decode_data"></a>

#### Decode data
Data in the transactions, traces, and logs tables are all encrypted. For example:
- method_id, data in the transactions table
- method_id, input, output in the traces table
- topic1, topic2, topic3, topic4, data in the logs table

To decode this data into a readable form, we need an ABI (Application Binary Interface) that is usually provided with the contract's source code. Let's look at an example of an ERC20 contract's ABI:

```json
[
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "name": "from",
                "type": "address"
            },
            {
                "indexed": true,
                "name": "to",
                "type": "address"
            },
            {
                "indexed": false,
                "name": "value",
                "type": "uint256"
            }
        ],
        "name": "Transfer",
        "type": "event"
    }
]
```

The result after decoding will get the table `ethereum decoded.erc20_evt_transfer` below:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjE4fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_evt_transfer.png" alt="erc20_evt_transfer" width="100%"></a></p>

The table is decoded from the logs table via ABI, you can see other decoded tables in the `ethereum_decoded` directory

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.ethereum_decoded.erc20_evt_transfer" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_evt_transfer_diagram.png" alt="erc20_evt_transfer_diagram" width="100%"></a></p>

#### Get information from the contract

For convenience, Chainslake needs to get more information from the contracts after decoding, these information tables are placed in the `ethereum_contract` folder. For example, the `erc20_tokens` table below:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjExfSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_tokens.png" alt="erc20_tokens" width="100%"></a></p>

The table contains information such as name, symbol, total supply of the contract. The contract address is taken from the `erc20_evt_transfer` table:

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.ethereum_contract.erc20_tokens" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_tokens_diagram.png" alt="erc20_tokens_diagram" width="100%"></a></p>


## Building insight tables<a name="insight_data"></a>

Raw data tables, decoded tables and contract tables are the raw materials that when combined according to a certain logic will form an insight table suitable for data query purposes. I will take the example of the `ethereum_balances.token_transfer_hour` table:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjE4N30sInR5cGUiOiJxdWVyeSJ9LCJkaXNwbGF5IjoidGFibGUiLCJ2aXN1YWxpemF0aW9uX3NldHRpbmdzIjp7fX0=" target="_blank"><img src="/assets/images/posts/2025-08-15/token_transfer_hour.png" alt="token_transfer_hour" width="100%"></a></p>

This is a table containing information about the balance changes of all wallets, all tokens on Ethereum every hour, including the following information:
- block_date, block_hour: Time information (indexed)
- wallet_address: Wallet address
- token_address: Token address, 0x if the token is Native ETH
- symbol: Token symbol
- amount: Number of tokens changed in the wallet in 1 hour

To build this table we need data from the transactions, traces, erc20_evt_transfer, erc20_tokens tables, the diagram is as follows:

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.ethereum_balances.token_transfer_hour" target="_blank"><img src="/assets/images/posts/2025-08-15/token_transfer_hour_diagram.png" alt="token_transfer_hour_diagram" width="100%"></a></p>

You can see the Source code of this table after clicking on the image above, I will briefly explain the logic of this table:

```sh
frequent_type=hour
list_input_tables=${chain_name}_decoded.erc20_evt_transfer,${chain_name}.traces,${chain_name}.transactions,${chain_name}_contract.erc20_tokens
output_table=${chain_name}_balances.token_transfer_hour
erc20_event_table=${chain_name}_decoded.erc20_evt_transfer
erc20_token_table=${chain_name}_contract.erc20_tokens
transactions_table=${chain_name}.transactions
traces_table=${chain_name}.traces
re_partition_by_range=block_date,key_partition
partition_by=block_date
write_mode=Append
number_index_columns=7
```

In the header of the Code we have the table configuration including:
- frequent_type: Indicates the frequency of updating the data of the table
- list_input_tables: List of tables as data input
- output_table: Name of the table to be written
- erc20_event_table, erc20_token_table, transactions_table, traces_table: Assign names to the input tables to be used in the query below
- re_partition_by_range: Indicates the columns to be sorted
- partition_by: Indicates the column to be partitioned
- write_mode: Data writing mode
- number_index_columns: Number of columns to be indexed in the table

In the body of the Code is the table construction logic written in SQL. Transfer token data is taken from the `erc20_evt_transfer` table, combined with `erc20_tokens` to get the name, symbol, decimals of the token, then grouped by block_hour, wallet_address, token_address to calculate the number of tokens changing per hour. For Native ETH, it is necessary to take from the traces table (only take successful transactions) and transactions (to get transaction fees).

## Conclusion<a name="conclusion"></a>

Although the article is quite long, I have introduced the most important parts of Chainslake's Ethereum data. I hope that from here you can explore and exploit Chainslake's data yourself. See you again!