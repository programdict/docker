https://docs.docker.com/engine/reference/builder/
## 基础知识
* 每条保留字指令都必须大写字母且后面至少一个参数
* 指令从上到下，顺序执行
* "#"表示注释
* 每条指令都会创建一个镜像层并对镜像进行提交

## 大致流程
* docker从基础镜像运行一个容器
* 执行一条指令并对容器做出修改
* 执行类似docker commit的操作提交一个新的镜像层
* docker再基于刚提交的镜像运行一个新容器
* 执行dockerfile中的下一条指令直到所有指令都执行完成

## 常用命令
* WORKDIR 打开终端的落脚点
* ADD
将宿主机目录下文件拷贝到镜像且会自动处理url和解压tar压缩包（COPY+解压）
* COPY
拷贝文件或目录到镜像中
* CMD
指定容器启动后要干的事情
* Dockerfile可以多个CMD指令，只有最后一个生效，CMD会被docker run之后的参数覆盖
* RUN是在docker build时运行
* CMD是在docker run时运行
* ENTRYPOINT
也是用来指定一个容器启动时要运行的命令
类似CMD指令，但是不会被docker run后面的命令覆盖，
而且这些命令行参数会被当做参数送给ENTRYPOINT指令指定的程序

## demo(vim+ifconfig+java8)
```shell script
FROM centos:7
MAINTAINER zzyyzzyybs@126.com
ENV MYPATH /usr/local
WORKDIR $MYPATH
#安装vim编辑器
RUN yum -y install vim
#安装ifconfig命令查看网络IP
RUN yum -y install net-tools
#安装java8及lib库
RUN yum -y install glibc.i686
RUN mkdir /usr/local/java
#ADD 是相对路径jar,把jdk-8u171-linux-x64.tar.gz添加到容器中,安装包必须要和Dockerfile文件在同一位置
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java/
#配置java环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
EXPOSE 80
CMD echo $MYPATH
CMD echo success--------------ok
CMD /bin/bash
```
docker build -t centosjava8:1.5 .

## 虚悬镜像
```shell script
FROM ubuntu
CMD echo 'action is success'
```
docker build .  
docker image prune  

