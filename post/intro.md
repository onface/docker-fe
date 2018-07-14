# Web 前端学 Docker 用来干嘛？

## 前端构建环境

前端构建环境基于 `Node.js` 实现。在 `node` 上利用 `webpack` `fis3`  `gulp` 等工具进行编译。

多人维护一个项目或技术团队维护多个项目，会通过服务器编译代码。

> 小型团队维护少量项目可以在 Mac 或者 Windows 编译，不需要服务器也不需要 Docker。（当然有些 npm 包不支持 windows ）

服务器编译代码需要安装 `node` 和一些其他服务器软件，不同的项目所需的 `node` 版本不一定是一致的。当多个项目在同一个服务器编译时版本问题会非常麻烦。

> 而且还要跟运维同事说，a 项目需要 `node-8` ，b 项目需要全局安装某某 npm 包。

俗话说得好：本地开发快准狠，在线构建慢累烦。

---

## Docker 是“虚拟机”

简单的说 Docker 是一个可以**快速启动的高性能**虚拟机。且配置简单。

Docker 与普通的虚拟机软件不同的是，通过几行配置代码和一行命令就能快速启动虚拟环境。

[安装 Docker ](http://get.daocloud.io/#install-docker-for-mac-windows) 并配置[加速器](https://www.daocloud.io/mirror#accelerator-doc)后启动 Docker 在运行命令

```bash
docker run -it node:10-slim bash
```

接着会出现下载进度，但只会在第一次运行下载。下载完成后运行

```bash
node -v
# v10.6.0
npm -v
# 6.1.0
```

运行 `exit` 退出。

还可以切换到 `node-9`

```bash
docker run -it node:9-slim bash
```

因为 Docker 快速、高性能、支持环境丰富的特性，前端可以利用 node 快速切换开发环境进行构建。

前端只需要将所有环境配置和构建命令写在一个文件名为 `Dockerfile` 的文件中，告诉运维同事运行 Docker 。

`Dockerfile` 范本

```bash
FROM onface/node-yarn-fis3:9-slim
WORKDIR /app
COPY . /app/
    & yarn \
    & npm run online
MAINTAINER youremail@domain.com
```

> Windows 可以使用 Docker 启动 `linux` 环境运行 Node.js，以解决部分 npm 包不支持 windows 的情况。
