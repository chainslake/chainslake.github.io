---
title: Transformer
author: Lake
date: 2024-06-01
category: Chainslake
layout: post
---

### Why do we need Transfomer?

[Chainslake core](/pages/6-core) core provides a warehouse of raw data, transactions, logs, traces and decoded tables from protocols. They are all data source tables, and we can absolutely write SQL queries on Metabase directly on this tables, thereby building charts, metrics and dashboards. However, there are 2 reasons we should not do so:
- Source tables are very large in size, up to millions, even billions of records. Executing aggregate queries and analyzing data on these tables will require a large amount of computation and resources and take a lot of time.
- Results from queries cannot be reused for other queries, and every time data needs to be updated, the query must be re-executed, which causes huge waste.

Transformer was built to solve these problems, it uses a [DBT](https://docs.getdbt.com/docs/introduction) platform that allows building intermediate data tables from source data tables. Transformer uses SQL (or Python) language to build data transformations, which makes it very easy to use, you just need to write queries in Transformer and it will help you build tables necessary. [Incremental](https://docs.getdbt.com/docs/build/incremental-models-overview) function also allows updating data of intermediate tables instead of rerunning the entire table.

> Unlike Metabase, which uses Trino as the query engine. Transformer uses Spark, so there will be some differences in SQL Syntax. The reason we chose this design is because of the different use cases:
- Metabase serves end users so it needs Trino's fast query execution, multiple query support, and security capabilities.
- Meanwhile, Transformer is the back-end execution task, it needs Spark's ability to analyze and process large data.

Like the *Core* component, data transformation jobs (called models) are also scheduled and automated using Airflow, you can see more in the [Data pipeline](/pages/8-datapipeline) documentation.

### Build your first model
In this section, we will guide you through the steps to build the *monitor.daily_report* model. This is a model that allows you to statistics the number of records in each data source table by day. Based on this model, you can build statistical charts and metrics to monitor the speed and volume of data on each table. The full source code of the monitor model is available on the chainslake repository.
1. First, let's look at the data source tables in our data warehouse, they all have the following 4 columns:
    - *block_date*: Date of data appearing on the blockchain
    - *block_number*: Block number of data
    - *block_time*: Time when the block is verified
    - *updated_time*: The time the data is stored in the table on the warehouse.
2. For each data source table, we will compile a number of indicators such as: number of records, number of blocks and number of block date, from block, to block, from date, to date according to each day of updated time. To avoid having to rewrite this query for each table, we'll use a macro to create a function, let create new macro:
    - `macros/monitor/aggreate_by_updated_time.sql`
    ```sql
    {% macro aggregate_by_updated_time(table_name, alias_name) -%}
    with table_updated_date as (
        select *, cast(updated_time as date) as updated_date
        from {{table_name}}
    )

    select '{{alias_name}}' as table_name
        , count(*) as number_rows
        , count(distinct block_number) as number_blocks
        , min(block_number) as from_block
        , max(block_number) as to_block
        , count(distinct block_date) as number_block_dates
        , min(block_date) as from_date
        , max(block_date) as to_date
        , updated_date

    from table_updated_date
    where updated_date < (select max(updated_date) from table_updated_date) 
    {% if is_incremental() %}
    and updated_date > (select max(updated_date) from {{this}} where table_name = {{alias_name}})
    {% endif %}

    group by updated_date

    {%- endmacro %}
    ```
    > This macro accepts as input the table name and the table's alternate name. It groups the table rows by updated date and calculates the metrics we need. You can see I have used the Incremental function through function is_incremental(), the meaning of which is that in case the table already exists, it will only be necessary to run the query on the next date instead of running the entire table again. This is an extremely useful function that greatly reduces the amount of unnecessary processing when updating data for the table.
2. Next we need to create the *ethereum_monitor* schema folder in the *models* folder and declare the schema in the *dbt_project.yml* file: 
    - `dbt_project.yml`
    ```yml
    models:
        transformer:
            ethereum_monitor:
                +schema: ethereum_monitor
    ```
3. Create model *ethereum_monitor.daily_report* and declare the model in the *schemas.yml* file of *ethereum_monitor*:
    - `models/ethereum_monitor/schemas.yml`
    ```yml
    version: 2

    models:
        - name: ethereum_monitor.daily_report
    ```

    - `models/ethereum_monitor/ethereum_monitor.daily_report.sql`
    ```sql
    {{ config(
        alias = 'daily_report',
        partition_by = ['updated_date'],
        materialized ='incremental',
        file_format ='delta',
        incremental_strategy = 'append'
        )
    }}
    ```
    > You can learn more about config of the model in [here](https://docs.getdbt.com/docs/build/sql-models)
4. Now we will write the SQL query to create the *daily_report* table using the *aggreate_by_updated_time* macro we built. However, before doing that, we need to declare the data source tables in the *base_sources* folder:
    - `models/base_sources/ethereum_base_sources.yml`
    ```yml
    version: 2

    sources:
    - name: ethereum_origin
        description: "Ethereum original tables"
        tables:
        - name: transaction_blocks
        - name: blocks_receipt
    - name: ethereum
        description: "Ethereum raw tables"
        tables:
        - name: transactions
        - name: logs
        - name: traces
    - name: ethereum_decoded
        description: "Ethereum decoded tables"
        tables:
        - name: erc20_evt_transfer
        - name: erc721_evt_transfer
        - name: erc1155_evt_transfersingle
        - name: erc1155_evt_transferbatch
        - name: uniswap_v2_evt_swap
        - name: uniswap_v3_evt_swap
    - name: ethereum_contract
        description: "Ethereum contracts"
        tables:
        - name: erc20_tokens
        - name: erc721_tokens
        - name: erc1155_tokens
        - name: uniswap_v2_info
        - name: uniswap_v3_info
    ```

    - `models/ethereum_monitor/ethereum_monitor.daily_report.sql`
    ```sql
    select * from (
    {{
        aggregate_by_updated_time(source('ethereum_origin', 'transaction_blocks'), 'origin.transaction_blocks')
    }}
    )

    union all
    select * from (
    {{
        aggregate_by_updated_time(source('ethereum_origin', 'blocks_receipt'), 'origin.blocks_receipt')
    }}
    )

    union all
    -- tobe continue --
    ```
5. All done, commit the code and push it to your chainslake repository.

### Run your model 

6. Pull newest code from your chainslake repository, go to transformer directory and run command to create table in data warehouse:
    ```sh
    hadoop@node ~$ cd path/your/chainslake/transformer
    hadoop@node transformer$ dbt run --select ethereum_monitor.daily_report
    ```
7. 
