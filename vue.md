# vue

* idea:vuejs插件失效

>复制Vue Single File Component文件生成格式，新建一个名称（Vue Component），扩展（vue）的类型生成右键选项

## Soc原则（关注点分离）

网络通信：axios

界面跳转：vue-router

状态管理：vuex

## UI框架

>●Ant-Design: 阿里巴巴出品，基紆React的UI框架
>●ElementUI、 iview、 ice: 饿了么出品，基于Vue的UI框架
>●Bootstrap: Twitter推出的一个用于前端开发的开源工具包
>●AmazeUI:又叫"妹子UI”，一款HTML5跨屏前端框架
>
>●iview

## JavaScript构建工具

> ●Babel: JS编译工具，主要用于浏览器不支持的ES新特性，比如用于编译TypeScript
> ●WebPack:模块打包器，主要作用是打包、压缩、合并及按序加载

## 前端为主的MV时代

>● MVC (同步通信为主) : Model、 View、 Controller
>●MVP (异步通信为主) : Model、 View、 Presenter
>●MVVM (异步通信为主，: Model、 View、 ViewModel

为了降低前端开发复杂度,涌现了大量的前端框架，比如: AngularJS(模块化)、 React（虚拟DOM）、 Vue.js(集大成者) 、EmberJS等，这些框架总的原则是先按类型分层，比如Templates、Controllers、 Models, 然后再在层内做切分

## 787原则

学习vue必须知道它的7个属性, 8个方法，以及7个指令。

### el属性

。用来指示vue编译器从什么地方开始解析vue的语法，可以说是一个占位符。

### data属性

。用来组织从view中抽象出来的属性，可以说将视图的数据抽象出来存放在data中。

### template属性

。用来设置模板，会替换页面元素， 包括占位符。

### methods属性

。放置页面中的业务逻辑, js方法- 般都放置在methods中

### render属性

。创建真正的Virtual Dom

### computed属性

。用来计算

### watch属性

。watch:function(new,old){}
。监听data中数据的变化
。两个参数，一个返回新值，一个返回旧值,

### 计算属性







## 插槽













## 安装node.js

### 插件

>Vue Language Features (Volar)[代码提示]
>
>TypeScript Vue Plugin (Volar)
>
>

* node.js

```shell
node -v
v16.13.1
npm -v
8.1.2
#获取registry镜像
npm config get registry
#设置registry镜像
npm config set registry=镜像地址
```

### npm工具使用

* npm安装element(ui框架）

>npm i element-ui -s



* npm安装cnpm（淘宝加速器）

>npm install cnpm -g  
>
>-g :全局安装

![image-20220712165934551](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202207121659697.png)



* cnpm 安装vue-cli

>cnpm install vue-cli -g



* 创建项目模板类型

>vue list

![image-20220712170419645](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202207121704696.png)



* npm安装webpack（打包工具）

```shell
---下载地址admin/appdata/Roaming/npm---
npm install cnpm -g
npm install pnpm -g
npm install webpack -g
#客户端
npm install webpack-cli -g						  
#查看版本号 
webpack -v					
#查看版本号
webpack-cli -v                                     
```



### 初始化项目

```
--> vue init webpack myvue                           
-- 初始化一个项目
【vue初始化项目失败Failed to download repo vuejs-templates/webpack: connect ETIMEDOUT 20.205.243.166:443】
原因一：
可能是你网络不好，换个网络即可
原因二
没有安装webpack，在cmd窗口中输入npm install webpack -g命令即可
原因三
设置一下cmd代理，set http_proxy=http://自己的代理服务器IP:自己的代理服务器端口
? Project name myvue                              --项目名称
? Project description A Vue.js project            --项目描述
? Author sunny                                    --项目作者
? Vue build standalone                            -- 项目构建(运行时编译)
? Install vue-router? No						
? Use ESLint to lint your code? No				
? Set up unit tests No							
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recommended) no
   vue-cli · Generated "myvue".
--> cd myvue
--> npm install
--> npm audit fix                                 -- 错误修复
--> npm run dev                                   -- 运行
--> webpack                                       -- 项目打包
--> (cnpm)npm install vue-router --save-dev       -- 安装路由插件
```





```
#vite构建项目
npm init vite@latest
@vue-cli构建项目


yarn create vite vue --template vue
```







### idea调试vue项目

>需先在终端启动项目
>
><img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202211021642183.png" alt="image-20221102164216088" style="zoom: 50%;" />
>
>配置调试信息，debug启动项目即可
>
><img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202211021645664.png" alt="image-20221102164508598" style="zoom:50%;" />



