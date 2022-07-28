# Gitee

### git常用命令

```
gut clone 仓库地址					 -- 克隆指定仓库到本地
```

```shell
#获取远程更新
$git fetch origin    
##把更新的内容合并到本地分支
$git merge origin/master 
#查看全部分支
git branch -a
#切换分支
git checkout -b deveoper origin/deveoper
#再次确认当前分支
git branch
git add .
git commit  -m"备注"                   -- 上传更新统一操作 
git push
git log                               -- 日志，可进行撤回
```

```
git push -f origin master			  -- 强制更新
```

## 上传gitee图片无法显示问题

#### 1. 腾讯云cos图床

![image-20220520103708729](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202205201037796.png)

### github

>git config --global http.sslVerify false                      -- 关闭ssl校验

#### 2. github图床

>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
>
>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
