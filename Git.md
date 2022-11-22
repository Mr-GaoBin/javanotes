# Git使用相关

## git常用命令

>### 创建新分支dev
>
>git branch dev
>
>### 查看分支，创建成功
>
>git branch -a
>
>### 切换分支
>
>git checkout dev 
>
>### 设置本地分支追踪远程分支
>
>### 设置后，以后再push时，只需 git push 命令即可
>
>git push --set-upstream origin dev
>
>### 版本回退
>
>git reset
>
>git reset --hard c1bde88481c2d5de047052956062420283be0a36



```shell
#克隆指定仓库到本地
gut clone 仓库地址					 
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

## 



### Git工作流程

>- 克隆 Git 资源作为工作目录。
>- 在克隆的资源上添加或修改文件。
>- 如果其他人修改了，你可以更新资源。
>- 在提交前查看修改。
>- 提交修改。

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202211222223450.png" alt="image-20221122222318378" style="zoom:50%;" />



### Git 工作区、暂存区和版本库

>- **工作区：**就是你在电脑里能看到的目录。
>- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
>- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202211222225066.png" alt="image-20221122222528994" style="zoom:67%;" />





## 部分图床配置

#### 1. 腾讯云cos图床

![image-20220520103708729](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202205201037796.png)

### github

>git config --global http.sslVerify false                      -- 关闭ssl校验

#### 2. github图床

>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
>
>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





## --------------------------------------------------------《=练习=》-------------------------------------------------------











