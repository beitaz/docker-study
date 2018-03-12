# 学习配置 Docker Rails 开发环境

* 创建项目
```SHELL
$ rails new . -B -f -d postgresql
$ docker build -t quoyi .
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

