# **dockerfile构建镜像（多阶段构建）**



```
FROM maven:3.8.3-adoptopenjdk-8 as cistage
MAINTAINER lihaifeng <li7hai26@gmail.com>
ADD ./pom.xml pom.xml
ADD ./src src/
RUN mvn clean package

FROM adoptopenjdk:8u292-b10-jdk-hotspot
COPY --from=cistage target/app.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```

