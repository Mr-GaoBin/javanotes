# **docker安装oracle**

docker run --restart=always -d -p 1521:1521 -e ORACLE_DATABASE=SHBOM -e ORACLE_PASSWORD=P@88w0rd -e APP_USER=SHBOMUSER -e APP_USER_PASSWORD=P@88w0rd -v oradata:/opt/oracle/oradata --name oracle gvenzl/oracle-xe:21-full