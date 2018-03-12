# Docker 学习项目

不建议从头开始手动构建 docker 镜像：
  1. 自己构建的镜像大小远超官方镜像
  2. 官方镜像会做很多优化且会随时更新
  3. 官方集成度更高
  4. 工作量问题

## Docker 命令

```SHELL
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

```SHELL
FROM <image_name> # 父镜像
CONTAINER <user_email> # 作者、维护者
USER <username> # 切换到 username 用户
RUN apt-get update # 执行 Ubuntu 更新命令
RUN 'whoami' # 查询当前用户
RUN /bin/bash -l -c 'pwd' # 调用 /bin/bash 并指定 -l (--login)登录，然后使用 -c (--command) 设置需要执行的命令字符串 
ENV WORKPATH dir_path # 设置变量 WORKPATH 并赋值为 dir_path 目录
RUN mkdir -p $WORKPATH # 递归创建 WORKPATH 所表示的目录
WORKDIR $WORKPATH # 设置默认工作目录为 $WORKPATH 指定的路径
EXPOSE ["$WORKPATH/public"] # 暴露静态资源目录
EXPOSE 3000 # 暴露（开放）3000 端口
ENTRYPOINT ["bundle", "exec"] # 预设入口点，简化命令： bundle exec rails server  =>  rails server
CMD ["rails", "server", "-b", "0.0.0.0"] # 启动容器时未设置调用命令，则默认调用此命令
```

**【注意】** 安装 RVM 时，需要重新登录 bash 或手动编译 bashrc，需要使用 `RUN echo 'source /usr/local/rvm/scripts/rvm' >> /etc/bash.bashrc`


## 删除`所有`后台进程、错误镜像
```SHELL
# 删除后台进程
$ docker rm --force `docker ps -a | grep -v "CONTAINER ID" | awk '{print $1}'`
# 删除错误镜像
$ docker rmi --force `docker images | grep "<none>" | awk '{print $3}'`
```

## docker-compose 集成

* 修改 `Gemfile` 文件：

```RUBY
# gem 'sqlite3' # 移除 sqlite3
gem 'pg' # 添加 postgresql
```

* 修改数据库配置 `config/database.yml` 文件：

```RUBY
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000
  username: postgres
  host: <%= ENV['POSTGRES_PORT_5432_TCP_ADDR'] %>
  port: <%= ENV['POSTGRES_PORT_5432_TCP_PORT'] %>

development:
  <<: *default
  database: quoyi_dev

test:
  <<: *default
  database: quoyi_test

production:
  <<: *default
  database: quoyi_pro
```
**【注意】** 配置文件中的 `POSTGRES_PORT_5432_TCP_ADDR` 和 `POSTGRES_PORT_5432_TCP_PORT` 会根据实际运行环境动态生成。

* 添加 `docker-compose.yml` 配置：
```YAML
app:
  build: .
  command: rails server -p 3000 -b '0.0.0.0'
  volumes:
    - .:/app
  links:
    - postgres
  ports:
    - "3000:3000"
postgres:
  image: postgres:latest
  ports:
    - '5432'
```

**【注意】** `app.links` 必须指定为数据库的名称 `postgres`；注意 `volumes` 指定方式。

* 镜像操作
```SHELL
$ docker-compose build
$ docker-compose run --rm app env # 查询运行时环境变量
$ docker-compose up # 运行镜像
$ docker-compose run app rake db:create db:migrate 
```