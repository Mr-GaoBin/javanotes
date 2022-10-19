# **docker安装mongodb**



```
docker run -d --restart=always --name mongo \
    -p 27017:27017 \
    -e MONGO_INITDB_ROOT_USERNAME=root \
    -e MONGO_INITDB_ROOT_PASSWORD=P@88w0rd \
    -v /data/mongo:/data/db \
    mongo
```



>docker-compose.yml
>
>```
>cat>docker-compose.yaml<<EOF
>version: '3.1'
>
>services:
>
>  mongo:
>    image: mongo
>    restart: always
>    ports:
>      - 27017:27017
>    environment:
>      MONGO_INITDB_ROOT_USERNAME: root
>      MONGO_INITDB_ROOT_PASSWORD: P@88w0rd
>
>  mongo-express:
>    image: mongo-express
>    restart: always
>    ports:
>      - 8081:8081
>    environment:
>      ME_CONFIG_MONGODB_ADMINUSERNAME: root
>      ME_CONFIG_MONGODB_ADMINPASSWORD: P@88w0rd
>      ME_CONFIG_MONGODB_URL: mongodb://root:root@mongo:27017/
>EOF
>docker-compose up -d
>```
>
>