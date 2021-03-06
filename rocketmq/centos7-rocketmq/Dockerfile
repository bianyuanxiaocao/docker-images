FROM centos:7

MAINTAINER 若虚 <slpcat@qq.com>

# Container variables
ENV \
    TERM="xterm" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    TIMEZONE="Asia/Shanghai"

COPY epel.repo /etc/yum.repos.d/
COPY java.sh /etc/profile.d/

# set timezone
RUN ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime

RUN \
    yum update -y && \
    yum install -y wget unzip && \
    wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.rpm && \
    rpm -ivh jdk-8u161-linux-x64.rpm && \
    rm jdk-8u161-linux-x64.rpm

# Rocketmq version
ENV ROCKETMQ_VERSION="4.1.0"
# Rocketmq home
ENV ROCKETMQ_HOME="/opt/rocketmq"
ENV JAVA_HOME="/usr/java/jdk1.8.0_161"
ENV JAVA_OPT=" -Duser.home=/opt/rocketmq"

RUN groupadd rocketmq && \
    useradd -g rocketmq -s /bin/bash -c RocketMQ rocketmq  && \
    chown -R rocketmq:rocketmq /home/rocketmq

# install from source
#RUN cd /opt && \
#    git clone https://github.com/apache/incubator-rocketmq.git
#WORKDIR /home/admin/incubator-rocketmq
#    mvn -Prelease-all -DskipTests clean install -U
#cd distribution/target/apache-rocketmq
# install from binary
RUN cd /opt && \
    wget https://mirrors.tuna.tsinghua.edu.cn/apache/rocketmq/${ROCKETMQ_VERSION}-incubating/rocketmq-all-${ROCKETMQ_VERSION}-incubating-bin-release.zip && \
    unzip rocketmq-all-${ROCKETMQ_VERSION}-incubating-bin-release.zip && \
    rm -rf rocketmq-all-${ROCKETMQ_VERSION}-incubating-bin-release.zip && \
    mv rocketmq-all-${ROCKETMQ_VERSION}-incubating rocketmq && \
    chown -R rocketmq:rocketmq /opt/rocketmq

USER rocketmq
WORKDIR /opt/rocketmq/bin
EXPOSE 9876 10909 10911
CMD  ["sh", "-c", ". ./play.sh; while sleep 10; do echo RocketMQ, GO ROCK; done"]
