# Save Image Size
## 1. 使用精简的基础镜像
如只在镜像内执行二进制文件,可以选择 `Alpine` 代替 `full-image`
  
## 2. 尽可能少的使用layers
一般来讲, layer 越少, image 越简单, size 越小, 可以尝试将多个相关的指令结合为一个 *`RUN`* 指令, 例如:
```dockerfile
RUN apt-get update \
 && apt-get install vim
``` 
代替
```dockerfile
RUN apt-get update
RUN apt-get install vim
```

## 3. 及时清理不需要的文件
比如需要配置 `cmake`, 首先需要下载 `cmake-3.21.1-linux-x86_64.tar.gz`, 然后解压配置. 在配置完成后,下载的源文件 `cmake-3.21.1-linux-x86_64.tar.gz` 就不再需要了就可以执行删除操作.
```dockerfile
RUN wget https://github.com/Kitware/CMake/releases/download/v3.21.1/cmake-3.21.1-linux-x86_64.tar.gz \
    && tar -zxvf cmake-3.21.1-linux-x86_64.tar.gz \
    && rm -f cmake-3.21.1-linux-x86_64.tar.gz \
    && mv cmake-3.21.1-linux-x86_64 /opt/cmake \
    && ln -s /opt/cmake/bin/cmake /bin/cmake
```

## 4. 别忘了使用`.dockerignore`
在`build docker image`过程中,同目录下的文件大小会影响到`build`的速度,而且会影响`size`, `.dockerignore` 的作用是在构建镜像时将