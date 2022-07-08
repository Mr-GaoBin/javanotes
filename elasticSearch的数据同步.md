# elasticSearch的数据同步



## 方案一：同步调用



![image-20220624134402002](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206241344081.png)





## 方案二：异步通知

* 解决同步调用存在的问题



![image-20220624134418575](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206241344617.png)



## 方案三：监听binlog

* 完全依赖canal中间件，解除服务之间的耦合，但是增大了Mysql的压力。



![image-20220624134437536](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206241344579.png)