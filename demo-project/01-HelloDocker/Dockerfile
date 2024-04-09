# 1.安装操作系统。
# FROM alpine # alpine 镜像

# 2.安装 JavaScript 运行时环境，即 NodeJS。
# 基于 aopine 的 NodeJs 20 镜像
FROM node:20-alpine

# 3.复制应用程序，依赖包，配置文件。
# COPY 命令后跟两个参数，第一个表示源路径，第二个表示目标路径
COPY index.js /index.js

# 4.执行启动命令，运行程序。
# CMD 命令后可以面跟一个命令组成的数组。也可以跟命令。
# CMD ["node" "/index.js"]
CMD node /index.js