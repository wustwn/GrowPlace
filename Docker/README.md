# Commands
`$ docker ps` // 查看所有正在运行容器

`$ docker stop containerId` // containerId 是容器的ID

`$ docker ps -a` // 查看所有容器

`$ docker ps -a -q` // 查看所有容器ID

`$ docker stop $(docker ps -a -q)` //  stop停止所有容器

`$ docker  rm $(docker ps -a -q)` //   remove删除所有容器

`$ sudo docker create --name container_name  image_name` // 由image新建一个container

`$ sudo docker run -it image_name /bin/bash` // 由image直接新建并运行一个container

`$ sudo docker run -it image_name --name container_name /bin/bash` // 由image直接新建并运行一个名为container_name的container

`$ sudo docker start\stop container_name` // 启动\停止container

```shell
1. $ sudo docker attach  container_name // 退出即停止，不会后台保持
2. $ sudo docker exec -it container_name bin/bash // 退出不会停止，保持up状态
```

`$ exit`  // 退出container，在container内执行

`$ docker save -o a.img.jar image_name` // docker由现有镜像输出为jar包

# Others
## How to search container's ip
```shell
1. enter container
2. cat /etc/hosts
```

## How to set up proxy for docker
1. Create a systemd drop-in directory for the docker service:
```shell
$ sudo mkdir -p /etc/systemd/system/docker.service.d
```

2. Create a file called /etc/systemd/system/docker.service.d/http-proxy.conf that adds the HTTP_PROXY environment variable:
```shell
[Service]    
Environment="HTTP_PROXY=http://proxy.example.com:443/ " \ 
            "HTTPS_PROXY=https://proxy.example.com:443/" \
            "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```

3. Flush changes:
```shell
$ sudo systemctl daemon-reload
```

4. Restart Docker:
```shell
$ sudo systemctl restart docker
```

5. Verify that the configuration has been loaded:
```shell
$ systemctl show --property=Environment docker
Environment=HTTPS_PROXY=https://proxy.example.com:443/
```

## 设置docker权限免sudo执行
1. 创建docker用户组
```shell
$ sudo groupadd docker
```
2. 添加当前用户至docker用户组
```shell
$ sudo usermod -aG docker $USER
```
3. 更新用户组权限
``shell
$ newgrp docker
```
4. Log out OS & Log in OS again
