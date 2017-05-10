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
docker run -e MASTER_HOSTNAME="mymaster" \
           -e MASTER="spark://mymaster:7077" \
           -e SPARK_PUBLIC_DNS="localhost" \
           --network=my_spark_cluster \
           -h mymaster \
           --name mymaster \
           -p "8080:8080" \
           -ti logitravel/spark-docker start-master
```

#### Run worker

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
           -ti logitravel/spark-docker start-worker
```

#### Run driver

```bash
docker run -e MASTER="spark://mymaster:7077" \
           -e MAIN_CLASS="com.logitravel.somesparkjob.Main" \
           -e DRIVER_MEMORY="1g" \
           -e EXECUTOR_MEMORY="1g" \
           -e EXECUTOR_CORES="8" \
           -e TOTAL_EXECUTOR_CORES="48" \
           -e DEPENDENCIES="org.apache.spark:spark-streaming_2.11:2.1.0,org.apache.spark:spark-streaming-kafka-0-8_2.11:2.1.0" \
           -e JOB_NAME="jobname" \
           -e JAR="http://example.com/repository/some-spark-job.jar" \
           --network=my_spark_cluster \
           --link mymaster:mymaster \
           -p "4040:4040" \
           -ti logitravel/spark-docker start-driver
```

### Deployment using docker swarm

#### Create the network
```
docker network create -d overlay spark --attachable
```

#### Create the service stack
```
docker stack deploy -c docker-compose.yml spark
```

#### Scale cluster
```
docker service scale spark_worker=100
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
