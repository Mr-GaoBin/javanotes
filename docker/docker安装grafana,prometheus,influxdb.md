

# **docker安装grafana,prometheus,influxdb**

# FAQ:

**prometheus.yml文件的默认配置可以从容器中挂载出来一份，也可以直接修改。这里有个坑，****就是在容器里面要访问宿主机的ip的话，不能写localhost****。方法是：要么将容器共享宿主机网络，这个不建议；要么就直接写宿主机的内网ip即可。**

**新版prometheus metrics地址发生改变 /prometheus/metrics**



**需要准备docker-compose.yaml    .env      prometheus.yml文件**

# prometheus.yml如下所示：

```shell
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'
    metrics_path: '/prometheus/metrics'
    static_configs:
    - targets: ['localhost:9090']
      labels:
        instance: prometheus

  - job_name: 'node-exporter'
    static_configs:
    - targets: ['10.91.249.44:9270']
      labels:
        instance: node-exporter
```



# docker-compose.yaml文件如下所示:

```shell
version: '3.1'

services:
 influxdb:
   image: influxdb:latest
   container_name: influxdb
   ports:
     - "8083:8083"
     - "8086:8086"
     - "8090:8090"
   environment:
     - INFLUXDB_DB=influxdb
     - INFLUXDB_ADMIN_USER=${INFLUXDB_USERNAME}
     - INFLUXDB_ADMIN_PASSWORD=${INFLUXDB_PASSWORD}
   volumes:
     - influxdb-storage:/var/lib/influxdb

 prometheus:
   image: prom/prometheus
   container_name: prometheus
   hostname: prometheus
   restart: always
   volumes:
     - ./prometheus.yml:/etc/prometheus/prometheus.yml
   command:
     - '--config.file=/etc/prometheus/prometheus.yml'
     - '--web.external-url=http://localhost/prometheus'
   ports:
     - "9090:9090"
   environment:
     - TZ="Asia/Shanghai"
     - PROMETHEUS_ADMIN_USER=${PROMETHEUS_USERNAME}
     - PROMETHEUS_ADMIN_PASSWORD=${PROMETHEUS_PASSWORD}
     
 grafana:
   image: grafana/grafana:latest
   container_name: grafana
   ports:
     - "3000:3000"
   environment:
     - GF_SECURITY_ADMIN_USER=${GRAFANA_USERNAME}
     - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_PASSWORD}
   depends_on:
     - influxdb
   user: "0"
   volumes:
     - grafana-storage:/var/lib/grafana
     - ./grafana-provisioning/:/etc/grafana/provisioning
volumes:
  influxdb-storage:
  grafana-storage:
```

# .env文件如下所示：

```shell
INFLUXDB_USERNAME=admin
INFLUXDB_PASSWORD=admin

GRAFANA_USERNAME=admin
GRAFANA_PASSWORD=admin

PROMETHEUS_USERNAME=admin
PROMETHEUS_PASSWORD=admin
```

# 运行compose命令并安装

```shell
docker-compose -f docker-compose.yml up -d
```



docker创建prometheus,grafana,mysqld-exporter

```shell
mkdir -p /data/prometheus

docker run -d \
 --name prometheus \
 --restart=always \
 -u root \
 -p 9090:9090 \
 -v /data/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml \
 -v /data/prometheus:/prometheus \
 -v /data/prometheus/conf:/etc/prometheus/conf \
 prom/prometheus

docker run -d \
 -p 3000:3000 \
 --restart=always \
 --name=grafana \
 -u root \
 -v /data/grafana:/var/lib/grafana \
 grafana/grafana

docker run -d \
  -p 9104:9104 \
  --link mysql:mysql \
  --restart=always \
  -e DATA_SOURCE_NAME="root:P@88word@(mysql:3306)/" \
  --name mysqld-exporter \
  prom/mysqld-exporter
```