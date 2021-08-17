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
**注意： 要删除文件的话需要在同一个*RUN* layer内进行删除，如果在不同layer进行删除，`container`内会删除，但是image size不会更改，依然是文件删除前的大小**

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
在Build docker image时,docker会先扫描当前目录下的文件, 若文件比较多,比较大就会非常耗时且无意义, 此时有两种解决方案: 
  - 删除unecessary files
  - 将Uncessary files/folders添加到`.dockerignore`内
    ```shell
    *.md
    !README.md
    ```
    > 除README.md外，所有其他md文件都被docker忽略

## 5. 使用`multi stage` 将`Image Build` 和 `Image Release` 分开
例如一种情况需要在`docker iamge A` 内 build , 需要在该 `docker image A` 内配置好依赖,比如 `base image` 需要使用`gcc`,需要配置`toolchain` 等, 但是在执行的时候并不需要这些依赖, `base image` 并不需要size比较大的`gcc`, 此时就可以使用 `multi stage`:
```Dockerfile
FROM gcc:latest as BASE
RUN ...
RUN ...

FROM ubuntu:20.04
COPY --from=BASE /root/FILE_A /root/FILE_A
COPY --from=BASE /root/FILE_B /root/FILE_B
```
`BASE` 则为 `stage 1 docker image`, 我们最终需要的是包含目标文件`FILE_A, FILE_B` 的`stage 2 docker image` 镜像
在 `docker build -t . target_image:1.0` 构建完成后, `docker images` 会出现*悬空*镜像,即`name none` & `tag none` 且没有关联 `container` 生成, 我们可以通过`docker image prune -f` 进行删除

 
