# 环境准备

## 语言环境
KubeLilin的后端代码由go语言编写，需要安装版本>=1.16的golang SDK

[GO SDK下载地址](https://go.dev/dl/)

KubeLilin的操作UI使用React的TypeScript版本进行编写，样式组件使用了蚂蚁金服的中台解决方案 AntDeignPro,需要安装版本>=18的NodeJS

[NodeJS 下载地址](https://nodejs.org/en/)

[AntDeignPro 文档地址](https://pro.ant.design/)

## 项目启动
### 服务端源码下载
 ``` shell
 git clone  https://github.com/KubeLilin/kubelilin.git
 ```

 
 ### 数据库初始化
 kubelilin使用MySQL作为服务端持久化数据库，默认使用的的版本的>=5.7的长期支持版本
 在scripts/mysql/init 中找到DDL_sgr_pass.sql 并执行其中的建表语句完成数据库的初始化，建表完成后，执行同级目录的sgr_pass_base_init.sql 完成平台基础数据例如:菜单等的初始化操作。

 ### 服务端启动
在完成前置的go sdk 的安装后，首先为了避免一些网络问题，我们需要先为go配置代理。

``` shell
goproxy
https://goproxy.io/zh/

七牛云
https://goproxy.cn

阿里云
https://mirrors.aliyun.com/goproxy/

$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,direct

执行完毕后，可以查看一下设置后的go环境变量
go env
如果列出的结果中出现了你刚才配置的代理，则代表已经成功配置
GOPROXY="https://goproxy.cn,direct"
```

代理配置完成后，在终端中进入到 go.mod 的目录,为项目同步依赖.
``` shell
cd src
// 下载依赖
go mod download
//同步依赖包，添加需要的，移除多余的
go mod tidy
```
代码依赖拉取完成后，需要编辑config-dev.yml配置文件，更改配置文件中数据连接信息，配置文件中默认是如下的配置, ${}在yml配置文件中可以用来读取环境变量
(注：yaml文件本身并不具备这个特性，这个特性是由yoyoGo框架提供的)，语法为 ${envName:默认值}，如果可以读到环境变量则会采用环境变量的值，否则会用冒号后的默认值，你可以通过把你的数据库连接信息写入环境变量，或者直接写到yml文件中皆可。
kubelilin支持多配置文件，真正读取哪个配置文件由启动时的环境变量决定的，例如dev环境下会读config_dev,test环境下会读取config_test。
``` yml
  datasource:
    pool:
      init_cap: 2
      max_cap: 5
      idle_timeout: 5
    db:
      name: db1
      type: mysql
      url: ${PAAS_DB_CONN:tcp(127.0.0.1)/sgr_pass?charset=utf8mb4&loc=Local&parseTime=True}
      username: ${PAAS_DB_USER:root}
      password: ${PAAS_DB_PWD:root}
      debug: true
```
配置好数据库后，在src目录下执行
``` shell
go build
```
编译成功后会出现一个名为 kubelilin的可执行文件，直接运行kubelilin即可。

也可以使用IDE打开源码，运行main文件中的main方法