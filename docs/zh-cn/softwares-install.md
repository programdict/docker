## tomcat
```shell script
# 启动
# tomcat
docker run -d -p 8080:8080 --name=t1 tomcat
# webapps删除，webapps.list改为webapps
docker exec -it t1 /bin/bash
rm -r webapps
mv webapps.list webapps
# 实用版
docker pull billygoo/tomcat8-jdk8
docker run -d -p 8080:8080 --name=mytomcat billygoo/tomcat8-jdk8
```

## mysql
```shell script
# 简单命令
docker pull mysql:5.7
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d --name=mysql mysql:5.7
# mysql查看字符集
show variables like 'character%'
# 实战命令
docker run -d -p 3306:3306 --privileged=true\
 -v /root/mysql/log:/var/log/mysql\
 -v /root/mysql/data:/var/lib/mysql\
 -v /root/mysql/conf:/etc/mysql/conf.d\
 -e MYSQL_ROOT_PASSWORD=123456\
 --name mysql\
 mysql:5.7
 # my.cnf
 [client]
 default_character_set=utf8
 [mysqld]
 collation_server = utf8_general_ci
 character_set_server = utf8
 # 重启
 docker restart myql
```

## redis
```shell script
# 启动
docker run -p 6379:6379 --name myredis --privileged=true\
 -v /root/redis/redis.conf:/etc/redis/redis.conf\
 -v /root/redis/data:/data
 -d redis:6.0.8 redis-server /etc/redis/redis.conf
 
 
#分片集群
docker run --name redis-node2 --net host --privileged=true\
 -v /data/redis/share/redis-node2:/data\
 -v /root/redis/node2/redis.conf:/etc/redis/redis.conf\
 -d redis redis-server /etc/redis/redis.conf

#配置文件
port 6381
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
requirepass dxy123
masterauth dxy123

# 构建集群
redis-cli --cluster create 49.4.5.89:6381 49.4.5.89:6382 117.78.46.209:6381 117.78.46.209:6382\
 --cluster-replicas 0
# 修改密码
config set masterauth dxy123
config set requirepass dxy123
config rewrite

#常用命令
cluster info
cluster nodes

#增加从节点
redis-cli --cluster add-node 117.78.46.209:6383 117.78.46.209:6381 --cluster-slave --cluster-master-id d8c226df6ff5aed00ce90a6cb84d37d8409952d7
redis-cli --cluster add-node 117.78.46.209:6383 117.78.46.209:6381 --cluster-slave --cluster-master-id d8c226df6ff5aed00ce90a6cb84d37d8409952d7 -a dxy123
#
```
redis分片集群
1.创建多份配置文件
bind 0.0.0.0
protected-mode no
#requirepass dxy123
#masterauth dxy123
port 6381
cluster-enabled yes
cluster-config-file nodes.conf
cluster-announce-ip 49.4.5.89
cluster-announce-bus-port 16381
cluster-node-timeout 5000
appendonly yes
2.创建多个实例
docker run --name redis-node1 --net host --privileged=true\
 -v /data/redis/share/redis-node1:/data\
 -v /root/redis/node1/redis.conf:/etc/redis/redis.conf\
 -d redis redis-server /etc/redis/redis.conf
3.创建集群
redis-cli --cluster create 49.4.5.89:6381 49.4.5.89:6382 117.78.46.209:6381 117.78.46.209:6382\
 --cluster-replicas 0
检查集群 redis命令行  cluster info，cluster nodes
4.配置文件放开requirepass、masterauth注释，重启docker实例

# nginx
https://blog.csdn.net/BThinker/article/details/123507820  
```shell script
docker run --name nginx --net=host --privileged=true -v /home/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/nginx/conf/conf.d:/etc/nginx/conf.d -v /home/nginx/log:/var/log/nginx -d nginx  
docker run --name nginx --net=host --privileged=true -v /server/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /server/nginx/conf/conf.d:/etc/nginx/conf.d -v /server/nginx/log:/var/log/nginx -d nginx  

docker run --name nginx --net=host  --privileged=true -v /root/nginx/conf.d:/etc/nginx/conf.d  -d nginx  

```

