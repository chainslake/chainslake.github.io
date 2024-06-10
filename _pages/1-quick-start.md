---
title: Quick start
author: Lake
date: 2024-06-01
category: Chainslake
layout: post
---

This guide will help you step-by-step deploy a blockchains data warehouse using the Chainslake framework. Please ensure that you have been added to our [chainslake repository](https://github.com/chainslake/chainslake) and have been given your own access key. 

### Setup guideline
1. First, you need to fork our chainslake repository to your organization on github. This will help you take control of building data models in your warehouse while continuing to receive our software updates.

2. Deploy system on your Infrastructure. You can deploy Chainslake in one of the following ways:
    - [Deploy on local](/pages/1-deploy-on-local): The simplest to install, use for development and testing.
    - [Deploy on AWS](/pages/2-deploy-on-aws): The best way to deploy in production, thanks to fault tolerance, scalability, and monitoring.
    - [Deploy on premise](/pages/3-deploy-on-premise): Deploy the system in an in-house environment with available hardware.

### Run first chain data pipeline
1. To run your first chain data pipeline, you need to create new __run__ and __dags__ for your chain. The fastest way is to copy our ethereum template.

    ```sh
    hadoop@node ~$ cd path/your/chainslake
    hadoop@node chainslake $ cp -r core/template core/run
    hadoop@node chainslake $ cp -r airflow-dag/dags_template airflow-dag/dags
    ```

2. You need to update config in `core/ethereum/application.properties`:
    - __access_key__: Paste your access key we gave you. Note that each access key can only work in only one system.
    - __rpc_list_for_trace__: RPC URL from Quicknode, you need to sign up and purchase a paid plan from the [Quicknode](https://www.quicknode.com/chains/eth) RPC service provider, then create a url endpoint for your chain, here ethereum.

3. Login to Airflow console (default in port 8080) and start DAGs: __ethereum_origin__, __ethereum__, __etherem_decoded__, __ethereum_contract__
    > Note that each DAG corresponds to a schema in the data warehouse, each task in the DAGs corresponds to a table in that schema. Tasks can run in parallel or sequentially depending on your arrangement.

### Query data and build first dashboard

### Next steps
You have now completed the installation a new Blockchains Data Lakehouse system using the Chainslake framework. To be able to use this system most effectively, the following documents will be very useful to you.
    - [Technical overview](/pages/5-technical-overview): This page will provide you with the most important core knowledge about this system from design architecture, technologies, components and cost estimates.
    - [System core](/pages/6-core): Describes how core jobs work and how to configure them. Core jobs include getting raw data from blockchain RPC, decoding data, getting contract information...
    - [Transformer](/pages/7-transformer): Instructions for building intermediate data tables from source data tables on Data warehouse.
    - [Data pipeline](/pages/8-data-pipeline): You will learn how to organize and schedule jobs in the system using Airflow.

Besides the technical side, you also need to read our [terms of use](/pages/9-terms-of-use) carefully.