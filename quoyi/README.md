# 学习配置 Docker Rails 开发环境

* 创建项目
```SHELL
$ rails new . -B -f -d postgresql # 以当前目录创建项目，-f 强制使用 -d 数据库 postgresql，并 -B 跳过 bundle install
$ docker build -t quoyi . # 以当前目录下的 Dockerfile 构建 -t 标签名为 quoyi 的镜像
$ docker-compose build
$ docker-compose up
$ docker-compose run app rake db:create db:migrate
```
浏览器打开 `localhost:3000` 检查是否正常显示。

* 若系统未安装 `postgresql` 时 `bundle install` 会报错：
```SHELL
$ brew install postgresql
$ brew services start postgresql # 后台运行 postgresql 服务
$ bundle install
```

