部署 Meteor 项目工具

[English Guide](https://github.com/meteorup/meteorupdev/blob/master/README.md)


meteorupdev 是一个 Meteor 项目快速部署工具，帮助开发人员快速、简单、安全的部署 Meteor 项目到各种服务器上。

meteorupdev 是由一个命令和一组服务组成的功能集合，可以通过 npm 方便的安装下载 meteorup 命令。

_（ps：注意：meteorup 暂时只支持在 linux 或 osx 系统中使用！）_


## 安装 meteorupdev 命令行工具

```bash
$ npm install meteorupdev -g
```

如果你没有安装Nodejs 和 npm 可以看这里[通过 NVM 安装 Nodejs 和 NPM](https://keymetrics.io/2015/02/03/installing-node-js-and-io-js-with-nvm/) 


**一键配置自己的服务器**

```bash
> $meteorupdev setup
```

meteorup 也支持在你自己的主机上部署 meteor 项目。使用 meteorupdev 实现这个私有部署，也很简单，只需要两个步骤。

第一步需要配置你的主机信息到本地项目的 package.json 文件中。然后通过 meteorupdev setup 命令完成服务器的环境安装和设置。setup 命令暂时只支持 CentOS 和 Ubuntu 服务器操作系统。

setup 命令会在您服务器上自动下载安装 NVM，nodejs，pm2，mongodb 等部署 meteor 需要的软件（mongodb 根据您的配置信息选择安装）。一台服务器只需要 setup 一次即可，重复在一台服务器上执行setup 命令不会有任何效果。

_（ps：setup 配置服务器会用时比较长，服务器网络环境好的情况下1分钟左右，一般需要5-10分钟左右，也会根据服务器网络情况有所不同。但一台服务器只需配置一次即可。）_

**一键部署Meteor项目到自己的服务器**

```
> $meteorupdev  push
```

经过上面的 setup 命令之后，你的服务器已经有了部署 meteor 项目的软件环境。可以通过 push 命令将你的 meteor 程序部署到你的服务器了。

每次运行 meteorupdev push 都会将你本地最新代码部署到服务器上，并覆盖之前同名的项目，但在服务器上会自动备份之前项目的代码和数据。你可以手动后退到之前部署过的任意版本。

**一键配置和部署之前，需要先完善 package.json 的部署信息**

在配置和部署之前需要设置package文件，以下就是package.json配置文件的样例。将一下内容加入到你项目的 package.json 文件中，如果没有 package.json 文件，请新建一个 package.json 文件。

**package.json**

```
        "server": {
            "host": "182.92.11.131",
            "username": "root",
            "//password": "password",
            "//":" or pem file (ssh based authentication)",
            "//": "WARNING: Keys protected by a passphrase are not supported",
            "pem": "~/.ssh/id_rsa",
            "//":" Also, for non-standard ssh port use this",
            "sshOptions": { "port" : 22 },
            "//":" server specific environment variables",
            "env": {}
        },
        "setup": {
            "//": "Install MongoDB on the server. Does not destroy the local MongoDB on future setups",
            "mongo": true,
            "//": "Application server path .  must in /usr /opt /home /alidata directory.",
            "path": "/usr/local/meteorup"
        },
        "deploy": {
            "//": "Application name (no spaces).",
            "appName": "best",
            "//": "Configure environment",
            "//": "ROOT_URL must be set to your correct domain (https or http)",
            "env": {
                "YJENV": "test", // customize environment
                "MONGO_URL": "mongodb://127.0.0.1:27017/best",
                "PORT": 8181,
                "ROOT_URL": "http://182.92.11.131:8181"
            }
        } 
```


**项目部署回撤**


登录服务器，进入 /usr/local/meteorup 目录，这个目录是由你的 package.json 配置文件中 setup 下 path 字段配置决定的，所以每次 push 到服务器上的项目都在这个目录下。通过 tree 你可以看到部署到服务器上的项目目录结构。

```
> $ ssh root@server
> $ cd /usr/local/meteorup
> 
> # tree -L 2
> 
> ├── app.json
> ├── current
> │   └── bundle
> ├── last20160622204939
> │   └── bundle
> ├── last20160622205649
> │   └── bundle
> ├── last20160622210322
> │   └── bundle
> ├── last20160622211151
> │   └── bundle
> ├── last20160622211636
> │   └── bundle
> └── tmp
```

这是个项目部署到服务器上的目录结构，可以看到所有部署过的项目都会一个备份镜像，current 目录下是当前项目的运行目录。如果要回撤到任意版本，只需要手动修改一个目录名为 current 即可。
例如：

```
> mv current backup
> $ mv last20160622204939 current
> $ pm2 start app.json
```

**查看部署到服务器上的状态**


```
> $ ssh root@server
> $ pm2 ls
> 
> ┌──────────┬────┬──────┬───────┬────────┬─────────┬────────┬──────────────┬──────────┐
> │ App name │ id │ mode │ pid   │ status │ restart │ uptime │ memory       │ watching │
> ├──────────┼────┼──────┼───────┼────────┼─────────┼────────┼──────────────┼──────────┤
> │ wechat   │ 0  │ fork │ 11557 │ online │ 5       │ 3D     │ 100.527 MB   │ disabled │
> │ webot    │ 1  │ fork │ 14845 │ online │ 0       │ 2D     │ 69.844 MB    │ disabled │
> └──────────┴────┴──────┴───────┴────────┴─────────┴────────┴──────────────┴──────────┘
```

服务器是通过 pm2 管理 node 服务的，所以你可以通过 pm2 的各种命令查看项目日志，管理你的项目服务。



## 部署完成的语音提示
config in package.json file. default would say “finished”
```bash
$ "notice": "Well done"
```


## 更新 meteorupdev 
 
```bash
$ npm install meteorupdev -g
```
 
## 问题

如果提示这样的错误： sudo: sorry, you must have a tty to run sudo

请通过 ssh 登录到服务器中，找到 /etc/sudoers 文件，注释 Default requiretty 配置信息即可

```
sudo vi /etc/sudoers
#Default requiretty
```


## License 协议

Meteorupdev is made available under the terms of the MIT License (MIT)

