# IDEA Plugins

>gitee                                                                                                  -- gitee仓库
>
>Free MyBatis plugin                                                                          -- mybatis代码生成插件
>
>MyBatisCodeHelperPro(Marketplace Edition)								  -- mybatis代码生成插件
>
>Translation  										 											 -- 翻译插件
>
>Chinese 																							-- 汉化插件

## 解决idea 导入maven项目卡住问题

### 产生原因

```
这个问题是从idea2020.升级到2020.2之后产生的，原因是idea升级到2020.2后加入了maven wrapper支持，会自动下载maven，当然下载速度可能只有几KB/s，所以会导致卡住
```



### 解决方案（三选一）

```
1.简单粗暴，删掉工程目录的.mvn目录，重新使用pom.xml导入
2.使用代理
3.复制自己的maven到~/.m2/wrapper/dists/apache-maven-3.6.3-bin/1iopthnavndlasol9gbrbg6bf2下
```













