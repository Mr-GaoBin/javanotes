# **docker安装nginx**



>## linux
>
>```
>docker rm -f nginx
>docker run -d -p 80:80 --name nginx nginx:1.21.3-alpine
>mkdir -p /data/nginx/dist
>docker cp nginx:/etc/nginx/nginx.conf /data/nginx/
>docker cp nginx:/etc/nginx/conf.d /data/nginx/
>docker cp nginx:/usr/share/nginx/html /data/nginx/
>docker rm -f nginx
>docker run -d -p 80:80 --name nginx \
> --restart=always \
> -v /data/nginx/nginx.conf:/etc/nginx/nginx.conf \
> -v /data/nginx/conf.d:/etc/nginx/conf.d \
> -v /data/nginx/html:/usr/share/nginx/html \
> -v /data/nginx/dist:/usr/share/nginx/dist \
> nginx:1.21.3-alpine
>```



>## windows
>
>```
>docker rm -f nginx
>
>docker run -d -p 80:80 --name nginx nginx:1.21.3-alpine
>mkdir -p ~/data/nginx/dist
>docker cp nginx:/etc/nginx/nginx.conf ~/data/nginx/
>docker cp nginx:/etc/nginx/conf.d ~/data/nginx/
>docker cp nginx:/usr/share/nginx/html ~/data/nginx/
>
>docker rm -f nginx
>
>docker run -d -p 80:80 --name nginx \
> --restart=always \
> -v ~/data/nginx/nginx.conf:/etc/nginx/nginx.conf \
> -v ~/data/nginx/conf.d:/etc/nginx/conf.d \
> -v ~/data/nginx/html:/usr/share/nginx/html \
> -v ~/data/nginx/dist:/usr/share/nginx/dist \
> nginx:1.21.3-alpine
>```