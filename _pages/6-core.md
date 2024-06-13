---
title: Chainslake core
author: Lake
date: 2024-06-01
category: Chainslake
layout: post
---
__Chainslake core__ is an important core component of the system, it is responsible for collecting data from blockchain RPCs, decoding data of protocols, and retrieving information from contracts. Core is located in the __core__ directory, is the only component of this system that does not share source code and is protected by a locking system. Any unauthorized copying and use of __Chainslake core__ without Chainslake's permission violates our [terms of use](/pages/9-terms-of-use) and is condemned.

### Organization
Chainslake core is located in the core directory of the chainslake repository, which includes:
    - chainslake-core.jar: Executable file of all jobs in Chainslake core
    - template: The folder contains executable script samples, the jobs of each chain are placed in different folders.
    - run: The directory contains system executable scripts, which can be cloned from the templates directory.
    > The run directory is located in .gitignore on the main Chainslake repository, you should remove it from .gitignore in your fork repository.
    - run/chain_name/application.properties: The config file is commonly used for all jobs of a chain.
    - run/chain_name/job_name.sh: script file that executes the job.
    > The executable file name and job name are set to be the same as the table or schema name that the job creates in the data warehouse.

### Core jobs
    - origin.transaction_blocks: Job get raw data from api [eth_getblockbynumber](https://ethereum.org/vi/developers/docs/apis/json-rpc/#eth_getblockbynumber).
    - origin.blocks_receipt: Job get raw data from api [eth_getBlockReceipts](https://www.quicknode.com/docs/ethereum/eth_getBlockReceipts)
    - origin.traces: Job get raw data from api [trace_block](https://www.quicknode.com/docs/ethereum/trace_block)
    - logs: Job extract `logs` table from `transaction_blocks` table
    - transactions: Job extract `transactions` table from `blocks_receipt` table
    - traces: Job extract `traces` table from 'origin.traces` table
    - decode_evt: Job decode event tables from `logs` table by using ABI from table `Protocol` in `evm_admin` database
    - decode_call: Job decode call tables from `traces` table by using ABI from table `Protocol` in `evm_admin` database
    - get_contract_info: Job get contract infor from RPC by using direction from table `ProtocolInfo` in `evm_admin` database

### Configuration
There are 2 types of config for each job that you need to know:
    - Spark configuration: Each job in core jobs is a Spark job so it uses [Spark configuration](https://spark.apache.org/docs/latest/configuration.html). Spark configuration is configured in the run script of each job, you need to note some important configurations below:
        - master: This configuration tells which cluster manager the spark job will be run on. You should use __local[*]__ for dev environments and __yarn__ for product environments.
        - deploy-mode: __client__ if you have 1 node, and __cluster__ if you have multiple nodes.
        - driver-memory: memory supplied for driver
        - executor-memory: memory supplied for each executor
        - num-executors: The number of executors executing the job cannot be more than the number of nodes in the cluster
        - executor-cores: Number of cores (threads) processed per executor.
    - Application properties: You can configure application-specific configurations in the __application.properties__ file or separately in each executable script with prefix: __spark.app_properties__. For example, to configure the number of partitions you can configure in the __application.properties__ file by adding `max_number_partition=1` or in the executable script by adding: `--conf "spark.app_properties.max_number_partition=10"`. Configuration located in the execution script will have higher priority. Configuration properties included:
        - access_key: The key controls the job run
        - secret_key: Comes with access key for protection
        - chain_name: name of chain
        - rpc_list: list of RPC URLs, you can use free RPC urls on [chainlist](https://chainlist.org/chain/1)
        - quicknode_rpc_list: list of Quicknode RPC URLs
        - max_number_partition: Maximum number of partitions in 1 processing
        - number_block_per_partition: Number of blocks in 1 partition
        - max_time_run: Number of runs. If this number is 0, the job will run continuously until all data is processed.
        - run_history: Configuration indicates whether to try to process past data or not.

Some notes for you when configuring job:
    - Jobs when first launched will start processing data from the highest block, and will continue to run the next blocks in subsequent runs. If config `run_history=true` job will simultaneously process data from the heightest block to block 0.
    - Each partition will be processed by 1 processing core (thread) of the executor and the results will be written to a file on storage. You should adjust the number of blocks in a partition appropriately for each chain according to needs and should not change it. Too many blocks in a partition will increase processing latency because jobs will wait until there are enough blocks in a partition to begin processing. For example, with ethereum, if you set the block number of a partition to 30, that means the processing delay of this chain is 30x12=360 seconds ~ 6 minutes. However, you should not set the number of blocks in a partition too low, because the number of partitions that can be processed at one time is limited by the number of executor cores of the cluster, and this will cause data fragmentation and make query slow later.
    - The total number of executor cores allocated to the job should be equal to the number of partitions. 
    - Tip: To determine the exact amount of memory to allocate for each job, you need to determine the amount of memory needed to process 1 partition. Let's test running 1 partition with minimum configuration of 1 core, 1 executor, 450m driver-memory, 1m executor-memory. Increment executor-memory if the Java heap space error persists until the job can execute successfully.

### Operation


## EVM Admin