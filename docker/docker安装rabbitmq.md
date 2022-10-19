# **docker安装rabbitmq**



>- linux环境
>
>```
>docker run -d --hostname rabbitmq --restart=always --name rabbitmq -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=P@88w0rd -v /data/rabbitmq:/var/lib/rabbitmq -p 5672:5672 -p 15672:15672 -p 25672:25672 rabbitmq:3-management
>```



>- windows环境(git bash)
>
>```
>docker run -d --hostname rabbitmq --restart=always --name rabbitmq -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=P@88w0rd -v ~/data/rabbitmq:/var/lib/rabbitmq -p 5672:5672 -p 15672:15672 -p 25672:25672 rabbitmq:3-management
>```