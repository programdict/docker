## 帮助启动类命令
```shell script
# 启动
systemctl start docker
# 停止
systemctl stop docker
# 重启
systemctl restart docker
# 查看状态
systemctl status docker
# 开机启动
systemctl enable docker
# 显示 Docker 系统信息，包括镜像和容器数等
docker info
# help命令
docker --help
docker 具体命令 --help
```

## 镜像类命令
```shell script
# 列出镜像
docker images
# 查找镜像
docker search name:tag
# 拉取镜像
docker pull name:tag
# docker使用磁盘空间信息
docker system df
# 删除某镜像
docker rmi imagename
# 删除所有镜像
docker rmi -f $(docker images -qa)
```

## 容器类命令
```shell script
# 列出
docker ps
# 启动
docker run
--name 名字
-d 守护式运行，后台运行
-i 交互式运行
-t 返回一个伪输入终端
-P 随机端口映射
-p 指定端口映射

# 退出
# 1.容器停止
exit
# 2.容器不停止
ctrl+p+q

docker start
docker restart
docker stop
docker kill
docker rm
docker rm -f $(docker ps -a -q)
docker ps -a -q | xargs docker rm
# 重要
# 日志
docker logs
docker logs -f --tail 100 容器ID
# 容器内进程查看
docker top
# 查看容器内部细节
docker inspect
# 进入正在运行的容器并以命令行交互
#  1.exit退出，容器不停止
docker exec -it
#  2.exit退出，容器停止
docker attach

# 容器内拷贝文件到主机上
docker cp 容器ID:容器内路径 目的主机路径
# 导入或导出容器
docker export 容器ID > 文件名.tar
docker import
cat 文件名.tar | docker import - 镜像用户/镜像名:tag
```