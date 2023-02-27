# pythonFlask

### 创建一个虚拟环境

创建一个项目文件夹，然后创建一个虚拟环境。创建完成后项目文件夹中会有一个 `venv` 文件夹：

```
$ mkdir myproject
$ cd myproject
$ python3 -m venv venv
```

在 Windows 下：

```
$ py -3 -m venv venv
```

在老版本的 Python 中要使用下面的命令创建虚拟环境：

```
$ python2 -m virtualenv venv
```

在 Windows 下：

```
> \Python27\Scripts\virtualenv.exe venv
```



### 激活虚拟环境

在开始工作前，先要激活相应的虚拟环境：

```
$ . venv/bin/activate
```

在 Windows 下：

```
> venv\Scripts\activate
```

激活后，你的终端提示符会显示虚拟环境的名称。

## 安装 Flask

在已激活的虚拟环境中可以使用如下命令安装 Flask：

```
$ pip install Flask
```

