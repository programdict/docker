## centos下安装
```shell script
# 1.centos7以上
cat /etc/redhat-release
# 2.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 3.安装gcc
yum -y install gcc
yum -y install gcc-c++
# 4.安装软件包
yum install -y yum-utils
# 5.设置stable镜像仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
或者
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 5.更新yum软件包索引
yum makecache fast
# 或centos8
dnf makecache

# 6.安装docker engine
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
systemctl start docker
systemctl status docker
6.1 安装镜像加速
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://hxqma6w1.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
# 7.运行测试
docker pull hello-world
docker run hello-world
# 8.卸载
systemctl stop docker
yum remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
