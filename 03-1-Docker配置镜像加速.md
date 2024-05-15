# Docker 配置镜像加速

Docker 的镜像加速有很多，这里以阿里云举例（如果没有阿里云账号，先注册一个账号）。

- 进入阿里云首页 -> 产品 -> 容器 -> 容器镜像服务 ACR -> 管理控制台 -> 左侧菜单栏 -> 镜像工具 -> 镜像加速器。

可以看到，阿里云为每个用户，生成了个性化的加速器地址。复制该加速器地址。

这里以修改 Windows 上的 Docker Desktop 客户端上的镜像加速为例：

- 进入 Settings，打开右侧 Docker Engine 选项卡，并把镜像加速地址配置在 json 文件中。

```json
{
  "registry-mirrors": ["https://[替换为你的加速地址域名].mirror.aliyuncs.com"]
}
```
