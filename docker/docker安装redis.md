# **docker安装redis**



>## redis单节点
>
>```
>REDIS_DATA=/data/redis
>mkdir -p $REDIS_DATA
>docker run --restart=always \
>  --name redis \
>  -p 6379:6379 -d \
>  -v $REDIS_DATA:/data \
>  redis:7.0-alpine --requirepass "P@88w0rd"
>```



>## redis-search
>
>```
>REDIS_DATA=/data/redis-search
>mkdir -p $REDIS_DATA
>docker run --restart=always \
>  --name redis-search \
>  -p 6380:6379 -d \
>  -v $REDIS_DATA:/data \
>  redislabs/redisearch:2.4.6
>```