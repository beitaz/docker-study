FROM ruby:latest
MAINTAINER quoyi007@gmail.com

# 备份源文件
#RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 修改 Ubuntu 镜像源
#RUN sed -i 's|http://archive.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list
#RUN sed -i 's|http://security.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list

# 安装运行 Rails 和 RubyGems 的基本依赖
RUN apt-get update && apt-get install -y build-essential nodejs

# 创建并设置工作目录
ENV WORKPATH /var/www/ruby
RUN mkdir -p $WORKPATH
WORKDIR $WORKPATH

# 安装项目依赖包
COPY Gemfile Gemfile.lock ./
#RUN gem install bundler && bundle install --jobs 20 --retry 5
RUN gem install bundler && bundle install

# 复制项目文件
COPY . ./

# 暴露（开放）端口
EXPOSE 3000

ENTRYPOINT ["bundle", "exec"]

# 通过容器 Container 启动时调用的命令，启动 Rails server
CMD ["rails", "server", "-b", "0.0.0.0", "-p", "3000"]
