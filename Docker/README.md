### how to search container's ip
```shell
1. enter container
2. cat /etc/hosts
```

`$ docker ps` // 查看所有正在运行容器

`$ docker stop containerId` // containerId 是容器的ID

`$ docker ps -a` // 查看所有容器

`$ docker ps -a -q` // 查看所有容器ID

`$ docker stop $(docker ps -a -q)` //  stop停止所有容器

`$ docker  rm $(docker ps -a -q)` //   remove删除所有容器


## How to set up proxy for docker
1. Create a systemd drop-in directory for the docker service:
```shell
$ sudo mkdir -p /etc/systemd/system/docker.service.d
```

2. Create a file called /etc/systemd/system/docker.service.d/http-proxy.conf that adds the HTTP_PROXY environment variable:
```shell
[Service]    
Environment="HTTP_PROXY=http://proxy.example.com:443/ ""HTTPS_PROXY=https://proxy.example.com:443/" "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
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
