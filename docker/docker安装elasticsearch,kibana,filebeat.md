# **docker安装elasticsearch,kibana,filebeat**





>elasticsearch单节点(shell)
>
>```
>docker run -d --name=es7 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elastic/elasticsearch:7.16.2
>
>mkdir -p /data/elasticsearch
>docker cp es7:/usr/share/elasticsearch/data /data/elasticsearch/
>docker cp es7:/usr/share/elasticsearch/logs /data/elasticsearch/
>docker rm -f es7
>
>docker run --restart=always -d --name=es7 \
>  --restart=always \
>  -p 9200:9200 -p 9300:9300 \
>  -e "discovery.type=single-node" \
>  -e ES_JAVA_OPTS="-Xms256m -Xmx256m" \
>  -v /data/elasticsearch/data:/usr/share/elasticsearch/data \
>  -v /data/elasticsearch/logs:/usr/share/elasticsearch/logs \
>  elastic/elasticsearch:7.16.2
>```
>
>



>elasticsearch单节点(windows) 使用git bash
>
>```
>docker run -d --name=es7 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elastic/elasticsearch:7.16.2
>
>mkdir -p ~/data/elasticsearch
>docker cp es7:/usr/share/elasticsearch/data ~/data/elasticsearch/
>docker cp es7:/usr/share/elasticsearch/logs ~/data/elasticsearch/
>docker rm -f es7
>
>docker run --restart=always -d --name=es7 \
>  --restart=always \
>  -p 9200:9200 -p 9300:9300 \
>  -e "discovery.type=single-node" \
>  -e ES_JAVA_OPTS="-Xms256m -Xmx256m" \
>  -v ~/data/elasticsearch/data:/usr/share/elasticsearch/data \
>  -v ~/data/elasticsearch/logs:/usr/share/elasticsearch/logs \
>  elastic/elasticsearch:7.16.2
>```



## kibana

>先准备kibana.yml
>
>```
>server.name: kibana
>server.host: "0"
>elasticsearch.hosts: [ "http://es7:9200" ]
>monitoring.ui.container.elasticsearch.enabled: true
>i18n.locale: "zh-CN"
>```

>docker安装kibana(mac/linux)
>
>```
>docker run --restart=always -d --name=kibana \
>  --restart=always --link es7:es7 \
>  -p 5601:5601 \
>  -v ~/data/kibana/kibana.yml:/usr/share/kibana/config/kibana.yml:rw \
>  elastic/kibana:7.16.2
>```

docker安装filebeat

>filebeat.yml
>
>```
>filebeat.inputs:
>- type: log
>  enabled: true
>  paths:
>    - /app/logs/gateway-bootstrap/gateway.log
>  fields:
>    source: gateway-bootstrap
>  json.keys_under_root: true
>  json.add_error_key: true
>
>- type: log
>  enabled: true
>  paths:
>    - /var/log/nginx/access.log
>  fields:
>    source: nginx-access
>
>- type: log
>  enabled: true
>  paths:
>    - /var/log/nginx/error.log
>  fields:
>    source: nginx-error
>
>filebeat.config:
>  modules:
>    path: ${path.config}/modules.d/*.yml
>    reload.enabled: false
>
>processors:
>  - add_cloud_metadata: ~
>  - add_docker_metadata: ~
>
>output.elasticsearch:
>  hosts: '${ELASTICSEARCH_HOSTS:192.168.192.201:9200}'
>  username: '${ELASTICSEARCH_USERNAME:}'
>  password: '${ELASTICSEARCH_PASSWORD:}'
>  index: "gw-%{[fields.source]}-*"
>  indices:
>    - index: "gw-gateway-bootstrap-%{+yyyy.MM.dd}"
>      when.equals:
>        fields.source: "gateway-bootstrap"
>    - index: "gw-nginx-access-%{+yyyy.MM.dd}"
>      when.equals:
>        fields.source: "nginx-access"
>    - index: "gw-nginx-error-%{+yyyy.MM.dd}"
>      when.equals:
>        fields.source: "nginx-error"
>
>setup.template.name: "gw-log"
>setup.template.pattern: "gw-*"
>setup.template.overwrite: true
>setup.template.enabled: true
>setup.ilm.enabled: false
>```



>docker安装filebeat
>
>```
>docker run -d --name=filebeat docker.elastic.co/beats/filebeat-oss:7.16.2-arm64
>docker cp filebeat:/usr/share/filebeat /data/
>docker rm -f filebeat
>docker run -d \
>  --name=filebeat \
>  --restart=always \
>  --net=host \
>  -v /data/filebeat:/usr/share/filebeat \
>  -v /app/logs:/app/logs \
>  -v /var/log:/var/log \
>  docker.elastic.co/beats/filebeat-oss:7.16.2-arm64
>```