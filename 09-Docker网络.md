# Docker 网络

## 一、Docker 网络是什么？

默认情况下，所有容器都是以 bridge 方式，连接到 Docker 的一个虚拟网桥上：

![docker网络](NodeAssets/docker网络.jpg)

查看 mysql 容器的网络配置，执行命令：

```shell
docker inspect mysql
```

```json
{
  "Networks": {
      "bridge": {
          "IPAMConfig": null,
          "Links": null,
          "Aliases": null,
          "MacAddress": "02:42:ac:11:00:02",
          "NetworkID": "9f6ed30840542d7409e6cf14aa78440dfec834bdf773d4e5ebc7319e031c5b87",
          "EndpointID": "d4407231d5536dcba121986a3c532d314bfa98b67a1e675849bd334726916653",
          "Gateway": "172.17.0.1",
          "IPAddress": "172.17.0.2",
          "IPPrefixLen": 16,
          "IPv6Gateway": "",
          "GlobalIPv6Address": "",
          "GlobalIPv6PrefixLen": 0,
          "DriverOpts": null,
          "DNSNames": null
      }
  }
}
```

发现 mysql 容器，处在 `172.17.0.1` 网卡中。

容器之间可以通过 ip 地址互相访问，但这种连接方式不好，因为停止容器再启动后，容器的 ip 地址是动态变化的。

加入 Docker 的自定义网络后，容器就可以通过容器名，互相访问了。

Docker 的网络操作命令如下：

| **命令**                  | **说明**                 | **文档地址**                                                 |
| ------------------------- | ------------------------ | ------------------------------------------------------------ |
| docker network create     | 创建一个网络             | [docker   network create](https://docs.docker.com/engine/reference/commandline/network_create/) |
| docker network ls         | 查看所有网络             | [docker   network ls](https://docs.docker.com/engine/reference/commandline/network_ls/) |
| docker network rm         | 删除指定网络             | [docker   network rm](https://docs.docker.com/engine/reference/commandline/network_rm/) |
| docker network prune      | 清除未使用的网络         | [docker   network prune](https://docs.docker.com/engine/reference/commandline/network_prune/) |
| docker network connect    | 使指定容器连接加入某网络 | [docker   network connect](https://docs.docker.com/engine/reference/commandline/network_connect/) |
| docker network disconnect | 使指定容器连接离开某网络 | [docker   network disconnect](https://docs.docker.com/engine/reference/commandline/network_disconnect/) |
| docker network inspect    | 查看网络详细信息         | [docker   network inspect](https://docs.docker.com/engine/reference/commandline/network_inspect/) |

## 二、Docker 网络创建

创建 docker 的 zetian 网卡，执行命令：

```shell
docker network create zetian
```

将 mysql 容器，连接到 zetian 网卡

```shell
docker network connect zetian mysql
```

也可以在容器创建时，直接将容器连接到网络，执行命令：

```shell
docker run --network 网卡名 容器名
```

在同一自定义网络下的其它容器中，可以使用 mysql 容器名，直接访问 mysql 容器，执行命令：

```shell
ping mysql
```
