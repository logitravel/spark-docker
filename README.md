# spark-docker

Apache Spark Docker Container

## Usage

### Build

```bash
docker build -t logitravel/spark-docker .
```

### Deployment

#### Create network

```bash
docker network create --driver bridge my_spark_cluster
```

#### Run master

```bash
# Environment variables
MASTER_HOSTNAME=master       # Master Hostname
MASTER=spark://master:7077   # Master URI
SPARK_CONF_DIR=/conf         # Configuration path
SPARK_PUBLIC_DNS=localhost   # Public DNS
```

```bash
docker run -e MASTER_HOSTNAME="mymaster" \
           -e MASTER="spark://mymaster:7077" \
           -e SPARK_PUBLIC_DNS="localhost" \
           --network=my_spark_cluster \
           -h mymaster \
           --name mymaster \
           -p "8080:8080" \
           -p "7077:7077" \
           -ti logitravel/spark-docker start-master
```

#### Run worker

```bash
# Environment variables
MASTER=spark://master:7077
SPARK_CONF_DIR=/conf
SPARK_WORKER_CORES=2
SPARK_WORKER_MEMORY=512m
SPARK_WORKER_PORT=8881
SPARK_WORKER_WEBUI_PORT=8081
SPARK_PUBLIC_DNS=localhost
```

```bash
docker run -e MASTER="spark://mymaster:7077" \
           -e SPARK_CONF_DIR="/conf" \
           -e SPARK_WORKER_CORES="2" \
           -e SPARK_WORKER_MEMORY="512m" \
           -e SPARK_WORKER_PORT="8881" \
           -e SPARK_WORKER_WEBUI_PORT="8081" \
           -e SPARK_PUBLIC_DNS="localhost" \
           --network=my_spark_cluster \
           --link mymaster:mymaster \
           -p "4040:4040" \
           -ti logitravel/spark-docker start-worker
```

### License

Copyright 2017 © Logitravel


Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

> [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
