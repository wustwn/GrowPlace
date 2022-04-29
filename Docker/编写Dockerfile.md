### FROM
- `FROM base_image`

- `FROM base_image : tag`

  - `base_image` 是在目标镜像中需要的基础组件或者基础服务等

  - `tag` 可以是镜像的版本

  - For Example:

  - `FROM node:10.15`
  - 即会在`build image`过程中先将`node 10.15 version` 从`/etc/docker/daemon.json`中配置的registry中pull下来,这样在build 成功的image中就包含了node的环境, 包括node, npm

### WORKDIR
- `WORKDIR url/path`

  - `url/path` 目标目录，若不设置则为默认目录。

- `WORKDIR /opt`
  - 启动container时默认目录为/opt，一般会搭配COPY等指令将文件复制到container当前目录下。

### COPY
- `COPY file/folder target_path`

  源文件可以是文件或者文件夹，若是文件夹则会将文件目录下所有文件拷贝进image，但是不包括文件夹本身。

- `COPY    folder_a      /a`
  假设folder_a目录下包含着file_1,file_2,那么在container内/a的目录结构为：
```shell
  |--a
     |—— file_1
     |—— file_2

  而不是：

  |--a
     |—— folder_a
         |-- file_1
         |-- file_2
```

### ADD
  - `ADD a.tar.gz target_path`
  - 与COPY类似，不过区别在于ADD会自动解压文件

### ENV
  - `ENV param_key = param_value`
    - 环境变量可以在build过程中设置，并且可以在启动container或在container内部进行再次赋值。

  - `ENV RABBITMQ_ADDRESS = localhost`

  - 需要注意的是：在定义ENV时可以进行多个变量同时定义，如：
  ```shell
  ENV A = a \
      B = b \
      C = c
  
  但是有一种情况不能这样使用，就是需要设置某环境变量为""时，此时需要单独ENV定义：

  ENV A = ""
  ENV B = ""
  ENV C = c \
      D = d \
      E = e
  ```

### ENTRYPOINT
 - `ENTRYPOINT ["/a.sh"]`
    - 可以使用此命令在启动container时执行一定的操作，比如上示命令即表示在run image时自动执行a.sh脚本，是经常会使用到的命令。

----

## 在Dockerfile 中编写安装 node, npm
最推荐的方式是通过安装`nvm` 进行安装，这样可以避免`dockerfile` 内的权限问题。
```dockerfile
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh | bash

ENV NVM_DIR=/root/.nvm

ENV NODE_VERSION=14.19.1

RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}

RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}

RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}

ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"


RUN node --version && npm --version

# avoid the sh.1 permission denied and other npm configruation issues
RUN npm config set user 0 && \

    npm config set unsafe-perm true && \

    npm config set strict-ssl false && \

    npm install --global vsce
```

