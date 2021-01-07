# zookeeper集群搭建
jdk版本最好使用1.8,再高版本zkCli.sh连接时会有问题
## zookeeper安装
```shell
# 将jar包拷贝到此目录
mkdir /usr/local/zk
cd /usr/local/zk

# 下载安装包
wget https://mirrors.bfsu.edu.cn/apache/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz

# 解压包
tar -zxvf zookeeper-3.4.14.tar.gz

# 配置相关目录
mkdir data
mkdir datLog
cd data
echo 1 > myid

# 配置zoo.cfg
cd ../zookeeper-3.4.14/conf
cp zoo_sample.cfg zoo.cfg
vim zoo.cfg

# 配置如下
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/usr/local/zk/data
dataLogDir=/usr/local/zk/dataLog
# the port at which the clients will connect
clientPort=2181
# servers
server.1=172.17.117.137:2888:3888
server.2=172.17.117.138:2888:3888
server.3=172.17.117.139:2888:3888
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1


# 其中server.n=ip:2888:3888 中n 即每台服务器配置的myid值,ip要与id对应

```

## 端口开放
```shell
# 以下命令我们只需使用开启防火墙, 设置开机启动, 添加端口(2181,2888,3888)即可

# 开启防火墙
systemctl start firewalld.service

# 防火墙开机启动
systemctl enable firewalld.service

# 关闭防火墙
systemctl stop firewalld.service

# 查看防火墙状态
firewall-cmd --state

# 查看现有的规则
iptables -nL

# 重载防火墙配置
firewall-cmd --reload

# 添加单个单端口
firewall-cmd --permanent --zone=public --add-port=81/tcp

# 添加多个端口
firewall-cmd --permanent --zone=public --add-port=8080-8083/tcp

# 删除某个端口
firewall-cmd --permanent --zone=public --remove-port=81/tcp

# 针对某个 IP开放端口
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.142.166" port protocol="tcp" port="6379" accept"
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.0.233" accept"

# 删除某个IP
firewall-cmd --permanent --remove-rich-rule="rule family="ipv4" source address="192.168.1.51" accept"

# 针对一个ip段访问
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.0.0/16" accept"
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.1.0/24" port protocol="tcp" port="9200" accept"

# 添加操作后别忘了执行重载
firewall-cmd --reload

# 查看指定级别的所有信息，譬如 public
#firewall-cmd --zone=public --list-all
```

## 集群启动
```shell
# 添加环境变量
export PATH=$PATH:/usr/local/zk/zookeeper-3.4.14/bin

# 3台机器都启动
zkServer.sh start

# 查看状态
zkServer.sh status

ZooKeeper JMX enabled by default
Using config: /usr/local/zk/zookeeper-3.4.14/bin/../conf/zoo.cfg
Mode: follower
```
