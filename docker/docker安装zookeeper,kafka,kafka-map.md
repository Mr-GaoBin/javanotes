# docker安装zookeeper,kafka,kafka-map



# 1.创建network

```
docker network create zkkafka
```

# 2.安装zookeeper,kafka

>创建zookeeper,kafka目录
>
>```
>mkdir zookeeper
>mkdir kafka
>```

## 2.1进入zookeeper目录

>docker-compose.yaml
>
>```
>version: "3"
>services:
>  zoo1:
>    image: zookeeper   
>    container_name: zoo1
>    restart: always
>    hostname: zoo1            
>    ports:
>      - 2181:2181 
>    environment:       
>      ZOO_MY_ID: 1   
>      ZOO_SERVERS: server.1=0.0.0.0:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181  
>  zoo2:
>    image: zookeeper
>    container_name: zoo2
>    restart: always
>    hostname: zoo2        
>    ports:
>      - 2182:2181
>    environment:
>      ZOO_MY_ID: 2
>      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=0.0.0.0:2888:3888;2181 server.3=zoo3:2888:3888;2181
>  zoo3:
>    image: zookeeper
>    container_name: zoo3
>    restart: always
>    hostname: zoo3     
>    ports:
>      - 2183:2181
>    environment:
>      ZOO_MY_ID: 3
>      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=0.0.0.0:2888:3888;2181
>networks:
>  default: 
>    external:
>      name: zkkafka
>```
>
>

## 2.2进入kafka目录

创建如下文件，创建完成后使用docker-compose up -d启动

>docker-compose.yaml
>
>```
>version: "3"
>services:
>  kafka1:
>    image: wurstmeister/kafka
>    restart: always
>    hostname: kafka1
>    container_name: kafka1
>    ports:
>      - 9092:9092
>    environment:
>      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka1:9092
>      KAFKA_LISTENERS: PLAINTEXT://kafka1:9092
>      KAFKA_ADVERTISED_HOST_NAME: kafka1
>      KAFKA_ADVERTISED_PORT: 9092
>      KAFKA_ZOOKEEPER_CONNECT: zoo1:2181,zoo2:2181,zoo3:2181
>    volumes:
>      - "/data/kafka/kafka1/:/kafka"
>    links:  # 连接本compose文件以外的container
>      - zoo1:zoo1
>      - zoo2:zoo2
>      - zoo3:zoo3
>
>  kafka2:
>    image: wurstmeister/kafka
>    restart: always
>    hostname: kafka2
>    container_name: kafka2
>    ports:
>      - 9093:9093
>    environment:
>      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka2:9093
>      KAFKA_LISTENERS: PLAINTEXT://kafka2:9093
>      KAFKA_ADVERTISED_HOST_NAME: kafka2
>      KAFKA_ADVERTISED_PORT: 9093
>      KAFKA_ZOOKEEPER_CONNECT: zoo1:2181,zoo2:2181,zoo3:2181
>    volumes:
>      - "/data/kafka/kafka2/:/kafka"
>    links:  # 连接本compose文件以外的container
>      - zoo1:zoo1
>      - zoo2:zoo2
>      - zoo3:zoo3
>
>  kafka3:
>    image: wurstmeister/kafka
>    restart: always
>    hostname: kafka3
>    container_name: kafka3
>    ports:
>      - 9094:9094
>    environment:
>      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka3:9094
>      KAFKA_LISTENERS: PLAINTEXT://kafka3:9094
>      KAFKA_ADVERTISED_HOST_NAME: kafka3
>      KAFKA_ADVERTISED_PORT: 9094
>      KAFKA_ZOOKEEPER_CONNECT: zoo1:2181,zoo2:2181,zoo3:2181
>    volumes:
>      - "/data/kafka/kafka3/:/kafka"
>    links:  # 连接本compose文件以外的container
>      - zoo1:zoo1
>      - zoo2:zoo2
>      - zoo3:zoo3
>
>networks:
>  default:
>    external:
>      name: zkkafka
>```
>
>

# 3.安装kafka-map

```
docker run -d \
    -p 8888:8080 \
    -v /data/kafka-map/data:/usr/local/kafka-map/data \
    -e DEFAULT_USERNAME=root \
    -e DEFAULT_PASSWORD=P@88w0rd \
    --name kafka-map \
    --link zoo1:zoo1 \
    --link zoo2:zoo2 \
    --link zoo3:zoo3 \
    --link kafka1:kafka1 \
    --link kafka2:kafka2 \
    --link kafka3:kafka3 \
    --network=zkkafka \
    --restart always dushixiang/kafka-map:latest
```

