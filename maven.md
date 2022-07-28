# maven









## jar无法下载手动导入

* 在项目中我们常常会遇到一种问题，我们依赖一个jar包，这个jar包可能是别人提供给你，也可能是自己下载的，但是我们用的是maven项目，这个jar包刚好又没有公共maven库去供你下载同步。那这种情况下就需要我们手动将jar包打包打本地仓库，再通过pom.xml引入。

>将jar下载至本地
>
>导入命令

```shell
mvn install:install-file -Dfile=D:/miiteen/miiteen-common-4.2.0.jar -DgroupId=com.miiteen -DartifactId=common -Dversion=4.2.0 -Dpackaging=jar
#jar包本地地址
-Dfile=D:/miiteen/miiteen-common-4.2.0.jar 
#jar包地址
-DgroupId=com.miiteen 
#jar名称
-DartifactId=common 
#jar包版本号
-Dversion=4.2.0 
#打包类型
-Dpackaging=jar
```





![image-20220710140157893](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202207101401992.png)