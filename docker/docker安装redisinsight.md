# **docker安装redisinsight**

```
REDIS_INSIGHT_PATH=/data/redisinsight
mkdir -p $REDIS_INSIGHT_PATH
sudo chmod 777 $REDIS_INSIGHT_PATH
sudo docker run --restart=always -d -v $REDIS_INSIGHT_PATH:/db \
  -p 8001:8001 --name redisinsight \
  redislabs/redisinsight:latest
```

