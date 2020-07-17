# Compose
Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务  
Compose 使用的三个步骤：  
* 使用 Dockerfile 定义应用程序的环境。
* 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
* 最后，执行 docker-compose up 命令来启动并运行整个应用程序。
    ```yaml
    #yaml配置示例
    version: '3'
    services:
    web:
        build: .
        ports:
    - "5000:5000"
        volumes:
    - .:/code
        - logvolume01:/var/log
        links:
    - redis
    redis:
        image: redis
    volumes:
    logvolume01: {}
    ```

## Compose安装
[发行版地址](https://github.com/docker/compose/releases)
```bash
[root@iZ2ze1le28b0mxc77tp939Z ~]#wget "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-Linux-x86_64"
[root@iZ2ze1le28b0mxc77tp939Z ~]#cp docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
[root@iZ2ze1le28b0mxc77tp939Z ~]# chmod +x /usr/local/bin/docker-compose
[root@iZ2ze1le28b0mxc77tp939Z ~]# ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
[root@iZ2ze1le28b0mxc77tp939Z ~]# docker-compose --version
```

## 使用
...待续