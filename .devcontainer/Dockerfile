FROM amazonlinux:2

# install epel
RUN amazon-linux-extras install -y epel

# yum update & install
RUN yum -y update \
    && yum -y install shadow-utils systemd tar unzip sudo git mariadb which

# install aws cli v2
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip && ./aws/install

# install python 3.8
# RUN yum install -y gcc zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel make libffi-devel \
#     && curl -LO https://www.python.org/ftp/python/3.8.16/Python-3.8.16.tgz \
#     && tar zxvf Python-3.8.16.tgz \
#     && cd Python-3.8.16 \
#     && ./configure --prefix=/usr/local/python \
#     && make \
#     && make install \
#     && ln -s /usr/local/python/bin/python3 /usr/local/bin/python \
#     && ln -s /usr/local/python/bin/pip3 /usr/local/bin/pip
RUN amazon-linux-extras install -y python3.8
RUN alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1 \
    && alternatives --install /usr/bin/python python /usr/bin/python2 1 \
    && alternatives --install /usr/bin/python python /usr/bin/python3 2 \
    && alternatives --install /usr/bin/pip pip /usr/bin/pip3.8 1

# create user
RUN useradd -u 1000 ec2-user && echo "ec2-user ALL=NOPASSWD: ALL" >> /etc/sudoers

# install node 16
USER ec2-user
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && source ~/.nvm/nvm.sh \
    && nvm install 16

## install CDK
RUN source ~/.nvm/nvm.sh && npm install -g aws-cdk

USER root

# init
CMD ["/sbin/init"]