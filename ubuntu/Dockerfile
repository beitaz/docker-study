FROM ubuntu:latest
MAINTAINER quoyi007@gmail.com

RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak
RUN sed -i 's|http://archive.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list
RUN sed -i 's|http://security.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list
# 安装运行 Rails 和 RubyGems 的基本依赖
RUN apt-get update
RUN apt-get install -y apt-utils curl build-essential nodejs --fix-missing --no-install-recommends

# 添加用户
ENV USERNAME bestar
RUN useradd -ms /bin/bash $USERNAME
RUN echo "$USERNAME ALL=(ALL)ALL" >> /etc/sudoers
#USER $USERNAME
RUN "su - $USERNAME"
# 下载 RVM
RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
RUN \curl -sSL https://get.rvm.io | bash -s stable
RUN 'whoami'
RUN 'cd ~'
RUN 'pwd'
RUN echo 'source /usr/local/rvm/scripts/rvm' >> /etc/bash.bashrc
RUN /bin/bash -l -c "rvm --version"
RUN /bin/bash -l -c "rvm install 2.5"
RUN /bin/bash -l -c "rvm use 2.5 --default"
RUN /bin/bash -l -c "gem sources --add https://gems.ruby-china.org --remove https://rubygems.org"
RUN /bin/bash -l -c "gem install bundler --nor-ri --no-rdoc"
RUN /bin/bash -l -c "gem install rails"

# 创建并设置工作目录
ENV WORKPATH /var/www/ruby
RUN mkdir -p $WORKPATH
WORKDIR $WORKPATH

# 安装项目依赖包
#COPY Gemfile .
#RUN gem install bundler && bundle install --jobs 20 --retry 5

# 复制项目文件
#COPY . .

# 预编译
#RUN bundle exec rake  assets:precompile db:create db:migrate db:seed RAILS_ENV=production

# 暴露生产环境的静态资源位置
EXPOSE ["$WORKPATH/public"]

# 暴露（开放）端口
EXPOSE 3000

# 预定义入口点(不用每次调用命令都输入 `bundle exec`)
ENTRYPOINT ["bundle", "exec"]

# 通过容器 Container 启动时调用的命令，启动 Rails server
#CMD ["rails", "server", "-b", "0.0.0.0", "-p", "3000"]
