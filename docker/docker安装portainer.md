# **docker安装portainer**



>## docker可视化工具
>
>```shell
>vi /usr/lib/systemd/system/docker.service
>
># ExecStart这一行替换为如下这行
>ExecStart=/usr/bin/dockerd -H unix://var/run/docker.sock -H tcp://0.0.0.0:2375 \
>         --containerd=/run/containerd/containerd.sock
>
># 安装2.x版本
>docker run -d --name portainer --restart=always \
>    -p 8000:8000 -p 9000:9000 -p 9443:9443 \
>    --name=portainer --restart=always \
>    -v /var/run/docker.sock:/var/run/docker.sock \
>    -v /data/portainer:/data \
>    portainer/portainer-ce:latest
>```