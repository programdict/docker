## docker compose
https://docs.docker.com/compose/compose-file/compose-file-v3/  
https://docs.docker.com/compose/install/  
```shell script
# 1.安装
# 下载
curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
# 或者
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-uname -s-uname -m -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker compose version
# 2.卸载
rm /usr/local/bin/docker-compose

```
docker-compose.yml  
service: 一个个应用容器实例，比如订单微服务、库存微服务、mysql容器、nginx容器或者redis容器  
project: 由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。  
