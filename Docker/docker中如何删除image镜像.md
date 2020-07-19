# docker中如何删除image镜像

docker 中删除images的命令是 docker rmi，但有时候执行此命令并不能删除 image，查看所有运行的容器进程：

![2020-04-14-15-32-22](http://qn.cawsct.com/2020-04-14-15-32-22.png)

我们在删除3c632001dbbb的镜像正在被a8b788d4c32b的container使用。

所以必须首先关闭container再删除该container，最后才可以删除这个镜像文件，操作如下：


1. 停止container

```
docker stop a8b788d4c32b
```

2. 删除容器

```
docker rm a8b788d4c32b
```

3. 删除镜像

```
docker rmi 3c632001dbbb
```


