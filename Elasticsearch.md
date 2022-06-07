# Elasticsearch



### Solr和ElasticSearch的区别

```
当单纯的对已有数据进行搜索时，Solr更快
当实时建立索引时，Solr会产生io阻塞， 查询性能较差，Elasticsearch具 有明显的优势
随着数据量的增加，Solr的搜索效率会变得更低，而Elasticsearch却没有明显的变化
```

#### elasticsearch安装及配置

>/elasticsearch/conf/elasticsearch.yml													-- 配置

```
# 开启跨域访问支持，默认为false
http.cors.enabled: true
# 跨域访问允许的域名地址
http.cors.allow-origin: "*"
```

>/elasticsearch/bin/elasticsearch.bat                                                        -- 启动

```
{
  "name" : "SUNNY",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "fD65dQdVS-aWUvXX9cavxg",
  "version" : {
    "number" : "7.6.1",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "aa751e09be0a5072e8570670309b1f12348f023b",
    "build_date" : "2020-02-29T00:15:25.529771Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}								   -- 访问http://localhost:9200
```



#### elasticsearch-head-master可视化界面

```
npm install 						-- 安装
```

```
npm run start 						-- 启动
								   -- 访问http://localhost:9100/
```

#### elasticsearchAPI测试工具

```
kibana/bin/kibana.bat				 -- 启动服务
kibana/conf/Kibana.yaml:
	i18n.locale: "zh-CN"              -- 语言配置   
    							    -- http://localhost:5601
```

Restful风格操作

```html
//查询
GET /pms/_doc/36

//删除
DELETE /.kibana_1

//删除指定id
DELETE /pms/_doc/-HWMxn8BdM6B2K6DkxDX

//会覆盖数据
PUT /pms/_doc/30
{
  "brandName": "鸿星尔克"
}

//只更新修改部分
POST pms/_doc/36/_update
{
  "doc":{
    "brandName": "nike"
  }
}

//添加
POST pms/_doc
{
  "doc":{
    "brandName": "sunny"
  }
}
```

>term查询是直接通过倒排索引|指定的词条进程精确的查找的!
>
>

#### elasticsearch-analysis-ik分词器

```html
将文件解压在/elasticsearch/plugin目录下
iK提供了两个分词算法：ik_smart和ik_max_word，其中ik_smart为最少切分，ik_max_word为最细粒度划分
```

自定义分词

```
/elasticsearch-analysis-ik/config/IKAnalyzer.cfg.xml文件中引入自定义词典(.dic)
```



### 生态圈

Lucene：是一套信息检索工具jar包，包含索引结构，读写索引工具，排序，搜索规则，不包含搜索引擎系统！

Solr和Elasticsearch都是基于Lucene做了一些封装和增强！

Elasticsearch是一个开源的高扩展的全文检索引擎，近乎实时存储，检索数据！

ELk（Elasticsearch，log，kibana）解压即用

### 分词器

### RestFull操作

### CRUD

###  springboot集成Elasticsearch

>Elasticsearch相关的pom依赖

```java
        <!--Elasticsearch相关依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
        </dependency>

        <!--  Springboot2.x中使用High Level REST Client
            配置：elasticsearch 替换 pom.xml中的：
            cluster-nodes: 127.0.0.1:9300 # es的连接地址及端口号  项目ES的IP地址
            cluster-name: elasticsearch # es集群的名称  ES的名称，打开ES页面后"cluster_name"参数的值
        -->
        <!-- elasticsearch -->
        <dependency>
            <groupId>org.elasticsearch</groupId>
            <artifactId>elasticsearch</artifactId>
            <version>7.13.0</version>
        </dependency>
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-high-level-client</artifactId>
            <version>7.13.0</version>
        </dependency>
        <!-- 解析网页 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.56</version>
        </dependency>

```



### 爬虫爬取数据

