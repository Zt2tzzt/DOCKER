# Docker 部署 Nginx

Ⅰ、查看 DockerHub 中 [Nginx 官方文档](https://hub.docker.com/_/nginx)。

Ⅱ、拉取 Nginx 镜像到本地，执行命令：

```shell
docker pull nginx
```

Ⅲ、查看本地镜像列表，执行命令：

```shell
docker images
```

Ⅳ、创建并运行 Nginx 容器，执行命令：

```shell
docker run -d \
  --name nginx \
  -p 80:80 \
  nginx
```

Ⅴ、查看容器，执行命令：

- 默认查看运行中的容器。
- 使用 `-a` 参数，要查看所有容器。
- 使用 `--format` 参数，格式化输出值。

```shell
docker ps
```

```shell
docker ps -a
```

```shell
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"
```

Ⅵ、停止容器，执行命令：

```shell
docker stop nginx
```

Ⅶ、再次启动容器，执行命令：

```shell
docker start nginx
```

Ⅷ、查看 Nginx 运行日志，执行命令：

- 使用 `-f` 参数，表示监听 Nginx 运行日志。

```shell
docker logs nginx
```

```shell
docker logs -f nginx
```

Ⅸ、进入 Nginx 容器，执行命令，

- 使用 `-it` 选项，表示添加一个可输入的终端。
- 使用 `bash` 参数，表示使用 bash 终端进行交互。

```shell
docker exec -it nginx bash
```

Ⅹ、删除容器，执行命令，

- 使用 `-f` 选项，表示强制删除（用于删除运行中的容器）：

```shell
docker rm mysql
```

```shell
docker rm mysql -f
```
