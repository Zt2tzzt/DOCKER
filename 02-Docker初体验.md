# Docker 初体验

## 一、Docker 实践

创建一个项目 01-HelloDocker，在其中创建一个文件 index.js

demo-project/01-HelloDocker/index.js

```javascript
console.log('我的第一个 docker 项目！哈哈哈')
```

### 1.Dockerfile 创建

在 docker 中运行以上这个 js 文件，需要执行以下步骤：

1. 安装操作系统。
2. 安装 JavaScript 运行时环境，即 NodeJS。
3. 复制应用程序，依赖包，配置文件。
4. 执行启动命令，运行程序。

将以上步骤，写入 Dockerfile 中，在项目根目录下，创建 `Dockerfile`。

demo-project/01-HelloDocker/Dockerfile

```dockerfile
# 1.安装操作系统。使用 alpine 镜像
# FROM alpine

# 2.安装 JavaScript 运行时环境，即 NodeJS。或直接使用基于 aopine 的 NodeJs20 镜像
FROM node:20-alpine

# 3.复制应用程序，依赖包，配置文件。
# COPY 命令后跟两个参数，第一个表示源路径，第二个表示目标路径
COPY index.js /index.js

# 4.执行启动命令，运行程序。
# CMD 命令后可以面跟一个命令组成的数组。也可以跟命令。
# CMD ["node" "/index.js"]
CMD node /index.js
```

### 2.Dockerfile 构建镜像

Ⅰ、在项目根目录，执行命令：

```shell
docker build -t 01-hello-docker .
```

表示从当前目录（项目根目录），构建一个名为 `01-hello-docker` 的镜像。

Ⅱ、查看构建好的镜像，执行命令，

```shell
docker image ls
# 或者
docker images
```

```shell
REPOSITORY        TAG       IMAGE ID       CREATED         SIZE
01-hello-docker   latest    7a5ed5051a7c   4 minutes ago   135MB
```

“TAG”，表示镜像的版本，如果构建时未指定，默认为 `latest`。

Ⅲ、在构建时，为镜像指定一个版本号。执行命令：

```shell
docker build -t 01-hello-docker:1.0.0 .
```

在 Docker Desktop 客户端的 Images 选项卡下，也可以看到构建好的 `01-hello-docker` 镜像。

### 3.创建并运行容器

根据构建好的镜像，创建并运行容器。执行命令：

```shell
docker run 01-hello-docker
```

如果你想要在另一个环境中，根据该镜像创建并运行容器，只需要把构建好的镜像文件复制过去，然后执行以上命令就可以了。

### 4.分享镜像

也可以把这个镜像文件，上传到 DockerHub 或 Harbor 镜像仓库中。

然后任何人都可以在任何地方，使用 `docker pull` 命令，来下载镜像文件。

## 二、在线 Docker 环境

[在线 Docker 环境](https://labs.play-with-docker.com/)中，使用 Docker 的各种功能。

- 比如构建镜像，使用容器等等。

执行命令，拉取别人上传在 dockerhub 的镜像：

```shell
docker pull geekhour/hello-docker
```

执行命令，使用镜像创建并运行容器。

```shell
docker run geekhour/hello-docker
```
