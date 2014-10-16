# Chianyu YunWeb Dockerfile 2014.09.11

# 加载base image
FROM ubuntu:12.04
# 签名
MAINTAINER yumiao <yumiao@chianyu.com>

# 准备目录
RUN mkdir -p /home/chianyu/jdk1.6.0_31
RUN mkdir -p /home/chianyu/apache-tomcat-7.0.40
RUN mkdir -p /home/chianyu/yunpan/disk

# 将开发仓库中的java、tomcat和yunweb文件夹拷贝到容器中指定目录下
ADD ./jdk1.6.0_31 /home/chianyu/jdk1.6.0_31
ADD ./apache-tomcat-7.0.40 /home/chianyu/apache-tomcat-7.0.40
ADD ./disk /home/chianyu/yunpan/disk

# 设置JAVA环境变量
ENV JAVA_HOME /home/chianyu/jdk1.6.0_31
ENV PATH $JAVA_HOME/bin:$PATH
ENV CLASSPATH .:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

# log日志目录
ENV LOG_DIR /home/chianyu/yunpan-log-dir
VOLUME /home/chianyu/yunpan-log-dir
# 运行时添加参数：-v /home/chianyu/yunpan-log-dir:/home/chianyu/yunpan-log-dir

# 安装OpenOffice文档转化服务
RUN apt-get update
RUN apt-get -y install xvfb
ADD ./openoffice3.2.1_64 /home/chianyu/openoffice3.2.1_64
RUN dpkg -i /home/chianyu/openoffice3.2.1_64/DEBS/*.deb

# 拷贝中文字体
ADD ./Fonts /usr/share/fonts
# 文泉驿-微米黑
RUN apt-get -y install ttf-wqy-microhei
# 文泉驿-正黑
RUN apt-get -y install ttf-wqy-zenhei
# 文泉驿-点阵宋体
RUN apt-get -y install xfonts-wqy
# 刷新字体缓存
RUN fc-cache -fv

# 安装Swftools
RUN apt-get -y install python-software-properties
RUN add-apt-repository -y ppa:guilhem-fr/swftools
RUN apt-get update
RUN apt-get -y install swftools

# jodconverter-core-3.0-beta-4.jar依赖它
ADD ./libsigar-amd64-linux.so /opt/openoffice.org/ure/lib/
ENV JAVA_OPTS -Djava.library.path=/opt/openoffice.org/ure/lib

# 对外开放8080接口
EXPOSE 8080

# 设置环境变量并运行tomcat
CMD  export JAVA_HOME=/home/chianyu/jdk1.6.0_31; /home/chianyu/apache-tomcat-7.0.40/bin/catalina.sh run