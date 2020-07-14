# Hello World
### 运行hello world 及参数解析
`docker run ubuntu:latest /bin/echo "hello world`  
* docker: Docker 的二进制执行文件。
* run: 与前面的 docker 组合来运行一个容器。
* ubuntu:15.10 指定要运行的镜像，Docker 首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。
* `/bin/echo "hello world"`: 在启动的容器里执行的命令

### 运行交互式容器  
`docker run -i -t ubuntu /bin/bash`  
* -t: 在新容器内指定一个伪终端或终端。
* -i: 允许你对容器内的标准输入 (STDIN) 进行交互

`root@bf025b34d82e:/#` 出现这个就代表进入了容器内部,此时我们可以查看容器内文件等, 可以通过exit或Ctrl+D来退出容器
```bash
root@bf025b34d82e:/# cat proc/version 
Linux version 4.18.0-147.5.1.el8_1.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 8.3.1 20190507 (Red Hat 8.3.1-4) (GCC)) #1 SMP Wed Feb 5 02:00:39 UTC 2020
root@bf025b34d82e:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### 启动容器(后台模式)
```bash
docker run -d ubuntu:latest /bin/sh -c "while true; do echo i have a dream; sleep 1; done"
e3a71278879678b54494b23c6f6dcaef7b141b42eb0f1f89dbd7f0e59a71440a  #这长串字符是 "容器id"

#继续 可以看到容器已经在运行
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
e3a712788796        ubuntu:latest       "/bin/sh -c 'while t…"   2 minutes ago       Up 2 minutes                            cool_lalande
```
* CONTAINER ID: 容器 ID。
* IMAGE: 使用的镜像。
* COMMAND: 启动容器时运行的命令。
* CREATED: 容器的创建时间。
* STATUS: 容器状态。
    > created（已创建）  
    > restarting（重启中）  
    > running（运行中）  
    > removing（迁移中）  
    > paused（暂停）  
    > exited（停止）  
    > dead（死亡）  
* ORTS: 容器的端口信息和使用的连接类型（tcp\udp）。
* NAMES: 自动分配的容器名称

```bash
# 通过上述的 容器id 或 容器名称 e3a712788796/cool_lalande 可以看到在不停的输出i have a dream
docker logs e3a712788796

i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
i have a dream
...
```

### 停止容器
```bash
# docker stop 容器id/名称来停止
docker stop e3a712788796

#继续docker ps可见已经没有运行的容器了
docker ps 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
