# Docker 学习项目

不建议从头开始手动构建 docker 镜像：
  1. 自己构建的镜像大小远超官方镜像
  2. 官方镜像会做很多优化且会随时更新
  3. 官方集成度更高
  4. 工作量问题

## Docker 命令

```shell
$ docker --version # 查询 docker 版本
$ docker images # 查询本地所有镜像
$ docker rmi <IMAGE ID> # 删除指定 IMAGE ID 的镜像
$ docker container ls # 查询本地所有容器
$ docker ps -a # 查询所有正在运行的容器
$ docker rm <CONTAINER ID> # 删除指定 CONTAINER ID 的容器
$ docker build -t TAG_NAME . # 按照所在目录下的 Dockfile 编译镜像，并设置标签 TAG_NAME
$ docker run -it IMAGE_NAME /bin/bash # 运行指定 IMAGE NAME 的镜像，并开启 bash
$ docker run -itP IMAGE_NAME # 运行指定 IMAGE NAME 的镜像，并暴露镜像的所有端口
```

## Dockerfile 配置项

```shell
FROM <image_name> # 父镜像
CONTAINER <user_email> # 作者、维护者
USER <username> # 切换到 username 用户
RUN apt-get update # 执行 Ubuntu 更新命令
RUN 'whoami' # 查询当前用户
RUN /bin/bash -l -c 'pwd' # 调用 /bin/bash 并指定 -l (--login)登录，然后使用 -c (--command) 设置需要执行的命令字符串 
ENV WORKPATH dir_path # 设置变量 WORKPATH 并赋值为 dir_path 目录
RUN mkdir -p $WORKPATH # 递归创建 WORKPATH 所表示的目录
WORKDIR $WORKPATH # 设置工作目录为 $WORKPATH 指定的路径
EXPOSE ["$WORKPATH/public"] # 暴露静态资源目录
EXPOSE 3000 # 暴露（开放）3000 端口
ENTRYPOINT ["bundle", "exec"] # 预设入口点，简化命令： bundle exec rails server  =>  rails server
CMD ["rails", "server", "-b", "0.0.0.0"] # 启动容器时未设置调用命令，则默认调用此命令
```

**【注意】** 安装 RVM 时，需要重新登录 bash 或手动编译 bashrc，需要使用 `RUN echo 'source /usr/local/rvm/scripts/rvm' >> /etc/bash.bashrc`
