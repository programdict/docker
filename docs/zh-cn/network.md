## docker 网络
docker0 虚拟网桥  
docker network ls  
bridge host none container 自定义 5种模式  
![图片](../_images/docker-network1.png)
![图片](../_images/docker-network2.png)
Alpine Linux  
docker link  
坑：容器间统一网桥，调用的端口号为容器内端口号，不是映射出来的端口号  
