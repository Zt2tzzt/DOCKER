# Docker 数据卷挂载

## 一、数据卷

数据卷（volume）是一个虚拟目录，是容器内目录，与宿主机目录之间映射的桥梁。用于操作容器内文件，或者方便迁移容器产生的数据。

以 Nginx 中的目录映射为例。宿主机目录，数据卷，与容器中目录的映射关系如下：

![数据卷](NodeAssets/数据卷.jpg)

基于 WSL2 的 Windows Docker Desktop 数据卷存放的位置如下：`\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes`。或在 `\\wsl$\docker-desktop-data\` 目录下查找。

Docker 数据卷的相关命令：

- `docker volume create`，创建数据卷。
- `docker volume ls`，查看所有数据卷。
- `docker volume rm`，删除指定数据卷。
- `docker volume inspect`，查看某个数据卷的详情。
- `docker volume prune`，清除数据卷。

## 二、Nginx 部署静态资源案例

需求：创建 Nginx 容器，其中的 html 目录下的 index.html 文件，查看变化。将静态资源，部署到 Nginx 的 html 目录

因为 Docker 中的镜像，只提供了软件运行的必须环境。

所以会发现，无法使用 vim 修改静态资源 index.html，因为环境中没有 vim 编辑器。也无法拷贝静态资源到容器的目录中。

Ⅰ、先删除 nginx 容器。执行命令：

```shell
docker rm nginx
```

```shell
zetian@cqc1000015167l3:~$ docker run -d \
        --name nginx \
        -p 81:80 \
        -v html:/usr/share/nginx/html \
        nginx
e8fa7dc37bd9e042f831046e4914b8eb8c3ce5ba6a0592c769a60bc80e5f30cd
```

Ⅱ、再创建 Nginx 容器的同时，挂载数据卷。执行命令：

在执行 `docker run` 命令时，使用 `-v 数据卷:容器内目录` 可以完成数据卷挂载。

当创建容器时，如果挂载了数据卷且数据卷不存在，会自动创建数据卷。

```shell
docker run -d \
    --name nginx \
    -p 81:80 \
    -v html:/usr/share/nginx/html \
    nginx
```

Ⅲ、查看创建的数据卷，执行命令：

```shell
docker volume ls
```

```shell
zetian@cqc1000015167l3:~$ docker volume ls
DRIVER    VOLUME NAME
local     c18b52b2e5a44f98189fe4b2be3fe1c7e47e6ad05a1a00680efbe5376934f33f
local     html
```

Ⅳ、查看该数据卷的详细信息。执行命令：

```shell
docker volume inspect html
```

```shell
zetian@cqc1000015167l3:~$ docker volume inspect html
[
    {
        "CreatedAt": "2024-04-11T05:58:25Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/html/_data",
        "Name": "html",
        "Options": null,
        "Scope": "local"
    }
]
```

Ⅴ、修改数据卷中 index.html 中的内容。容器中对应的 index.html 文件也会改变。

## 三、MySQL 查看数据卷

执行命令

```shell
docker inspect mysql
```

查看到的 mysql 容器数据卷挂载信息如下：

```json
{
  "Mounts": [
      {
          "Type": "volume",
          "Name": "c18b52b2e5a44f98189fe4b2be3fe1c7e47e6ad05a1a00680efbe5376934f33f",
          "Source": "/var/lib/docker/volumes/c18b52b2e5a44f98189fe4b2be3fe1c7e47e6ad05a1a00680efbe5376934f33f/_data",
          "Destination": "/var/lib/mysql",
          "Driver": "local",
          "Mode": "",
          "RW": true,
          "Propagation": ""
      }
  ],
}
```

发现 mysql 镜像在创建容器时，默认进行了数据卷的挂载。

- 这是为了将 mysql 保存的数据，映射到宿主机，避免 mysql 容器停止运行后的数据丢失。
- 也出于数据解耦的考虑，方便以后进行数据迁移

然而，mysql 容器默认挂载的数据卷，是一个匿名卷，名字非常长。

现在，要求基于宿主机目录（指定目录）实现 MySQL 数据目录、配置文件、初始化脚本的挂载（查阅官方镜像文档）。
