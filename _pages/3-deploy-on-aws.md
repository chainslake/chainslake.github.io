---
title: Install and configure on AWS
author: Lake
date: 2024-06-01
category: Chainslake
layout: post
---

### Prepare
1. IAM AWS Account can access to AWS console with policies:
    - `AmazonEC2FullAccess`
    - `AmazonElasticMapReduceFullAccess`
    - `AmazonS3FullAccess`
    - `AWSGlueConsoleFullAccess`
2. Role with name `EMR` with policy: `AmazonElasticMapReduceFullAccess`
3. Role with name `EMRInstanceProfile` with policy: `AmazonElasticMapReduceforEC2Role`

### Step by step

1. Login to your IAM AWS Account which you have just created above
2. Create VPC
    - In VPC console, click to __Create VPC__ button
    - Choose __VPC and more__ option in Resources to create
    - Fill `EMR` to name of VPC
    - Select 1 Availability Zones, 1 public subnets and 1 private subnets
    - Click to __Create VPC__ button to finish.
3. Create Bucket for datawarehouse on S3
    - In S3 console, access to Buckets page and choose __Create bucket__ button
    - Fill Bucket name (For example: companyname-chainslake) and click __Create bucket__ button
    - Inside Bucket, create two directory with name `warehouse` and `logs`
4. Setup Spark EMR cluster
    - In EMR console, click to __Create cluster__ button
    - Set name of cluster is __Spark__
    - Choose `emr-6.11.1` in EMR release selection
    - Check to __Spark 3.3.2__, __Hadoop 3.3.3__, and __Trino 4.1.0__ checkbox
    - Check to __Use for Spark table metadata__ checkbox
    - In __Cluster configuration__ section, let choose ec2 instance which you want. In my case, I choose `m5.xlarge` for Primary, Core, and Task node.
    - In __Cluster scaling and provisioning__ section, you can config size of your cluster. I recommend to use __Set cluster size manually__ and __Use Spot purchasing option Spot purchasing option__ for all task node, let start your cluster with 3 nodes includes 1 Primary, 1 Core and 1 Task node.
    - In __Networking__  section, please choose VPC and Public subnets which you have created in step 2
    - In __Cluster termination and node replacement__, let choose __Manually terminate cluster__ 
    - In __Cluster logs__, check __Publish cluster-specific logs to Amazon S3__ and fill `s3://[bucket-name]/logs` to Amazon S3 location. 
    - In __Software settings__ please add this config bellow:
    ```code
        [
            {
                "Classification": "yarn-site",
                "Properties": {
                    "yarn.nodemanager.resource.cpu-vcores": "50"
                }
            },
            {
                "Classification": "spark-hive-site",
                "Properties": {
                    "spark.sql.warehouse.dir": "s3://[bucket-name]/warehouse"
                }
            },
            {
                "Classification": "trino-connector-delta",
                "Properties": {
                    "hive.metastore": "glue"
                }
            },
            {
                "Classification": "trino-connector-delta",
                "Properties": {
                    "hive.metastore.glue.default-warehouse-dir": "s3://[bucket-name]/warehouse"
                }
            },
            {
                "Classification": "trino-config",
                "Properties": {
                    "query.max-memory-per-node": "4096MB"
                }
            },
            {
                "Classification": "delta-defaults",
                "Properties": {
                    "delta.enabled": "true"
                }
            }
        ]
    ```
    - In __Security configuration and EC2 key pair__, please choose your key pair for SSH
    - In __Identity and Access Management (IAM) roles__, you need to choose `EMR` for __Service role__ and `EMRInstanceprofile` for __Instance profile__, which are two roles that you have created in Prepare section.
    - Finally, click to __Create cluster__ button to start your cluster.
5. Create and config public ip
    - In EC2 console, access to __Elastic IP__ page in __Network & Security__ section
    - Click to __Allocate Elastic IP address__ button
    - Add name of volume is EMR in __Tag__ section
    - Click __Allocate__ button
    - In __Elastic IP addresses__ page, click on the newly created EMR ip and choose __Associate Elastic IP address__
    - In __Instance__ secction, find and select Primary node of Spark EMR cluster.
    - Check to __Allow this Elastic IP address to be reassociated__ checkbox
    - Click __Associate__ button to finish
    > Now you can access the Primary node of Spark EMR cluster via the newly created public IP
    ```sh
    $ ssh -i ./your_key.pem hadoop@[your-public-ip]
    ```
6. Create a stable volume and mount it to your Primary node.
    - In EC2 console, let choose __Volumes__ and click to __Create volume__ button
    - Config size of this volume about 120 Gb
    - Add name of volume is EMR in __Tag__ section
    - Click to __Create volume__ button
    - In Volumes page, you choose your EMR volume, click __Actions__ and choose __Attach volume__
    - You need to find and choose Primary node Instance in Instance section
    - Choose device is `/dev/sdc`
    - Click to __Attach volume__ button
    - Use ssh to access to Primary node of your Spark EMR cluster
    - Create new folder and mount it to your volume
    ```sh 
    hadoop@10.0.0.1 ~$ mkdir projects
    hadoop@10.0.0.1 ~$ sudo mount /dev/sdc /home/hadoop/projects/
    ```
    - Install git and clone project __chainslake__ from your repository
    ```sh
    hadoop@10.0.0.1 projects$ git clone git@github.com:YOUR_ORGANIZATION/chainslake.git
    ```
7. Config docker
    ```sh
    hadoop@10.0.0.1 ~$ sudo systemctl start docker
    hadoop@10.0.0.1 ~$ sudo usermod -aG docker $USER
    ```
    > You need to logout and re login your session to use docker command

8. Install Postgres and create databases
    - Install Postgressql
    ```sh
    hadoop@10.0.0.1 chainslake/postgres$ ./docker_run.sh
    ```
    - Create databases we need
    ```sh
    hadoop@10.0.0.1 chainslake/postgres$ docker exec -it postgres bash
    root@44a877ac8295:/# su postgres
    postgres@44a877ac8295:/$ psql
    postgres=# create database metabase
    postgres=# create database airflow_db
    postgres=# create database evm_admin
    postgres=# \q
    postgres@44a877ac8295:/$ exit
    root@44a877ac8295:/# exit
    ```
9. Install and setup Metabase
    ```sh
    hadoop@10.0.0.1 chainslake/metabase$ ./docker_run.sh
    hadoop@10.0.0.1 chainslake/metabase$ docker exec -it metabase bash
    ce3edf95af45:/# cd /plugins
    ce3edf95af45:/# wget https://github.com/starburstdata/metabase-driver/releases/download/4.1.0/starburst-4.1.0.metabase-driver.jar
    ce3edf95af45:/# exit
    hadoop@10.0.0.1 chainslake/metabase$ docker restart metabase
    ```
    - Access to Metabase UI in web browser with URL: `http://[your-public-ip]:3000` and create new admin account.
    > You can find lastest version of metabase plugin in [here](https://github.com/starburstdata/metabase-driver/releases)
10. Config domain and https
    - In EC2 console, access to __Security Groups__ page in __Network & Security__
    - Find Security group with name __ElasticMapReduce-master__
    - in __ElasticMapReduce-master__ click to __Edit inbound rules__ button
    - Add two new rules with type HTTP (port 80), HTTPS (port 443) and vpn (port 1184 UDP) with source is __Anywhere-IPv4__
    - Click __Save rules__ button
    - Install Nginx and Cerbot
    ```sh
    hadoop@10.0.0.1 ~$ sudo amazon-linux-extras install epel
    hadoop@10.0.0.1 ~$ sudo yum -y install certbot-nginx
    ```
    - Config nginx in file: `/etc/nginx/nginx.conf`
    ```conf
    http {
        server {
            server_name [YOUR_DOMAIN];
            client_max_body_size 100M;
            location / {
                proxy_pass http://127.0.0.1:3000;
                proxy_read_timeout 3600;
                proxy_connect_timeout 3600;
                proxy_send_timeout 3600;
            }
        }
    }
    ```
    ```sh
    # Reload nginx
    hadoop@10.0.0.1 ~$ sudo nginx -s reload
    ```
    - Config HTTPS
    ```sh
    hadoop@10.0.0.1 ~$ sudo certbot --nginx -d YOUR_DOMAIN -d YOUR_DOMAIN
    ```
    - Now you can try access to Metabase by your domain with https: `https://your-domain`
11. Install VPN
    ```sh
    hadoop@10.0.0.1 chainslake/install$ ./openvpn-install.sh
    ```
    - Create and download vpn client key to your local computer
    - In your local computer, you need install Open VPN client to connect server
    ```sh
    $ sudo openvpn --config ./client.ovpn
    ```
12. Install and run EVM admin
    ```sh
    hadoop@10.0.0.1 chainslake/evm-admin$ ./build.sh
    hadoop@10.0.0.1 chainslake/evm-admin$ ./docker-run.sh
    hadoop@10.0.0.1 chainslake/evm-admin$ docker exec -it evm_admin python manager.py migrate
    hadoop@10.0.0.1 chainslake/evm-admin$ docker exec -it evm_admin python manager.py createsuperuser
    ```
    - Now you can try to access to EVM admin by using private IP: `http://10.0.0.1:8000/admin`
    - Login to EVM admin page by using superuser account which you have create above
13. Install and run Transformer
    ```sh
    hadoop@10.0.0.1 chainslake/spark$ ./spark-transform-server.sh
    hadoop@10.0.0.1 chainslake/transformer$ ./build.sh
    hadoop@10.0.0.1 chainslake/transformer$ ./docker_run.sh
    ```
    - Now you can try to access to Transform doc server by using private IP: `http://10.0.0.1:8090`
14. Install and run Airflow
    ```sh
    hadoop@10.0.0.1 chainslake/install$ ./airflow.sh
    hadoop@10.0.0.1 chainslake/dag-airflow$ ./run.sh &
    ```
    - Now you can try to access to Airflow DAG by using private IP: `http://10.0.0.1:8080`
    - Login to Aiflow DAG page by using username `admin` and password in file `chainslake/dag-airflow/standalone_admin_password.txt`
15. Config Metabase connect to Trino
    - In Metabase page, login to Admin account
    - Go to Admin settings, Databases page, choose Add database
    - Select Database type is __Starburst__
    - Fill Display name you want
    - You fill `172.17.0.1` in host and `8889` in port
    - Type `delta` in catalog, `Trino` in username and blank in password
    - Click __Save__ button to finish.

