FROM centos:7

#复制file中的文件到 /root里
COPY file /root

#设置/root为工作路径
WORKDIR /root

#下载组件，给脚本添加可执行权限
RUN yum install -y wget initscripts tar\
    && chmod +x *.sh

#将start.sh添加到systemctl服务，并设置自启，以此初始化数据库
RUN mv start.service /usr/lib/systemd/system/\
    &&chmod 754 /usr/lib/systemd/system/start.service\
    &&systemctl enable start.service

#运行脚本安装配置apache，mariadb，php，wordpress
run sh install.sh

#暴露端口
EXPOSE  80

#启动容器时执行init，配合--privileged 可执行systemctl命令
CMD ["init"]
