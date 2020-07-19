# docker笔记

## docker架构

docker是C/S 架构的软件，跟mysql一样，他会启动一个守护进程。客户端和服务端可以运行在不同机器上，而docker的客户端就是docker的命令行，docker的服务端是一个JSON API服务。

对镜像每进行一次操作就会产生一个新的镜像层，即layer。他不会显示在镜像列表中，但是有一个imageID来标识它。好处是有几个镜像有共同部分时候，就没必要下载或者构建重复的部分。

## 命令行

1. 查看已有的镜像 docker images
2. 下载镜像 docker pull
3. 删除镜像 docker rmi
4. 运行镜像 docker run
5. 查看所有容器（包括隐藏停止的） docker ps -a
6. 删除停止的容器 docker rm
7. 停止运行的容器 docker stop

### docker如何删除image镜像

docker删除镜像命令是docker rmi, 但是有时执行命令并不能删除image，

![2020-04-14-15-32-22](http://qn.cawsct.com/2020-04-14-15-32-22.png)

我们在删除3c632001dbbb的镜像正在被a8b788d4c32b的container使用。所以必须首先关闭container再删除该container，最后才可以删除这个镜像文件，操作如下：

1. 停止container
docker stop a8b788d4c32b

2. 删除容器
docker rm a8b788d4c32b

3. 删除镜像
docker rmi 3c632001dbbb


> 我们一般不会直接手动去做，而是通过Dockerfile来做这些事情。

## Dockerfile

编写Dockerfile最重要的是认识指令，所有的指令都可以在Docker文档找到。

1. 通过Dockerfile构建镜像

```
docker build .
```
该命令会在当前目录下找到Dockerfile构建。这一个点代表当前目录。

2. FROM指令

```
FROM ubuntu
```
标识以何种镜像为基础

3. RUN命令

```
RUN npm i && npm run build
```

执行一些shell命令，比如安装依赖等。建议把命令尽量写到一行，因为一个指令就是一个镜像层。

4. CMD命令

```
CMD ['npm', 'start']
```

当我们通过docker run 直接启动容器时，他会直接执行运行npm run start这句命令。CMD用来指定容器默认执行的命令。

5. COPY与ADD命令

```
COPY . /code
```
这2个命令都是把文件复制到容器中。相比较而言，ADD是读取网络文件更加优秀，一般用COPY就够用了。

6. EXPORT

将本机80端口映射到容器内部的8080端口
```
EXPORT 80 8080
```

7. WORKDIR

切换目录，跟cd命令一样。

```
WORK DIR /code
```

### 以nuxt为主如何编写Dockerfile



## .dockerignore

.dockerignore 文件作用就是，里面的内容不管是在 docker build 过程中，还是 docker run 的过程中，有没有这个文件好像并没有什么很大的影响，存在感不强的一个文件。

当我们在 docker build 的过程中，首先会将指定的上下文目录打包传递给 docker引擎，而这个上下文目录中可能并不是所有的文件我们都会在 Dockerfile 中使用到，那么这个时候就可以在 .dockerignore 文件中指定在传递给 docker引擎 时需要忽略掉的文件或文件夹。

## 前端项目

我们在前端项目中，node_modules 文件夹在构建镜像过程中如果用不到，但是又异常庞大，那么向 docker引擎 传递其实是并没有必要的，这个时候就可以将 node_modules 文件夹加入 .dockerignore 文件中。

![加入前](https://img-blog.csdn.net/20180309202800407?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveHMyMDY5MTcxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

加入.dockerignore后：

![加入后](https://img-blog.csdn.net/20180309202943386?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveHMyMDY5MTcxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)