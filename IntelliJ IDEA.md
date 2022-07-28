# IntelliJ IDEA 

## IDEA Plugins

>gitee                                                                                                  -- gitee仓库
>
>Free MyBatis plugin                                                                          -- mybatis代码生成插件
>
>MyBatisCodeHelperPro(Marketplace Edition)								   -- mybatis代码生成插件
>
>MyBatisX								 														  -- mybatis代码生成插
>
>Translation  										 											 -- 翻译插件
>
>Chinese (Simplified) Language Pack											     -- 汉化插件
>
>Chinese 																							-- 汉化插件
>
>Atom Material Icons																		  -- 图标插件





## 操作面板优化

* target 目录

>idea 的maven项目，打包后,项目 目录 没有 显示target **点击左边工程的 设置，弹出图片的框，勾上 show Exculded Files**,就可以看到target 目录 了



#### 注释编辑

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206171128016.png" alt="image-20220617112852914" style="zoom: 50%;" />



#### 构建springboot项目连接超时

* 将默认的地址更改为aliyun地址

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206201012703.png" alt="image-20220620101207601" style="zoom: 50%;" />



#### 设置maven仓库默认地址

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206201021718.png" alt="image-20220620102142663" style="zoom:50%;" />

配置maven

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206201023481.png" alt="image-20220620102316409" style="zoom: 50%;" />



## 内存优化

#### **一、前言**

IDEA默认启动配置主要考虑低配置用户，参数不高（默认最低128m，最高512m），导致启动慢，然后运行也不流畅，这里我们需要优化下启动和运行配置；但是在工作中的电脑一般都是8G或者16G的运行内存，所以我们需要手动去修改默认的IDEA配置。

#### **二、手动修改IDEA配置**

#### **配置查看IDEA内存使用情况**

在 Settings -> Appearance & Behavior 设置窗口中，勾选 Show memory indicator 选项，然后主界面右下角会显示 Heap 总大小以及使用状况了。

在验证设置是否生效时候可以查看这里

![image-20220615111621416](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206151116544.png)

#### **修改IDEA配置**

打开 idea64.exe.vmoptions 配置文件，在Help -> Edit Custom VM Option...中设置

![image-20220615104513516](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206151045878.png)

默认设置



<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206151046111.png" alt="image-20220615104632070" style="zoom: 80%;" />

关键的三个参数的说明

> 1、-Xms 是最小启动内存参数
> 2、-Xmx 是最大运行内存参数
> 3、-XX:ReservedCodeCacheSize 保留代码占用的内存容量参数

建议手动设置参数值

电脑运行内存为8G的建议

```python3
-server
-Xms512m
-Xmx1024m
-XX:ReservedCodeCacheSize=300m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
```

电脑运行内存为16G的建议

```python3
-server
-Xms1024m
-Xmx2048m
-XX:ReservedCodeCacheSize=500m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
```

为什么初始内存也要设置512或1024M那么大？有文章这样说：此值可以设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存。

