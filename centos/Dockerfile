FROM centos

# Install needed packages for RVM
RUN yum -y update && yum -y install which curl gpg gcc gcc-c++ make
# yum -y groupinstall "Development Tools"

# Install RVM
RUN curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
RUN curl -sSL https://get.rvm.io | bash -s stable

RUN echo 'source /usr/local/rvm/scripts/rvm' >> /etc/bash.bashrc
 # Install Tiller through RVM
RUN /bin/bash -l -c "rvm install 2.5.0 && rvm use 2.5.0 --default && \
                 gem sources -a https://gems.ruby-china.org -r https://rubygems.org && \
                 gem install bundler --no-ri --no-rdoc && gem install rails"

RUN /bin/bash -l -c "yum install -y epel-release"
RUN /bin/bash -l -c "yum install -y --enablerepo=epel nodejs npm"
RUN /bin/bash -l -c "yum install -y yum-utils && yum-config-manager --enable epel && \
                     yum clean all && yum update -y"

RUN /bin/bash -l -c "yum install -y pygpgme && \
                     curl --fail -sSLo /etc/yum.repos.d/passenger.repo https://oss-binaries.phusionpassenger.com/yum/definitions/el-passenger.repo"

RUN /bin/bash -l -c "yum install -y nginx passenger || yum-config-manager --enable cr && yum install -y nginx passenger"


# Run in a login shell so RVM can set up Ruby paths etc.
CMD ["/bin/bash" , "-l" , "-c" , "rvm", "--version"]

