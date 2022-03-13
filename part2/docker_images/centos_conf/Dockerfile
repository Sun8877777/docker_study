FROM centos:centos7
RUN mkdir -p ~/new-dir1
RUN mkdir -p /etc/myconf
RUN echo "Some config data" >> /etc/myconf/my.conf

RUN useradd -ms /bin/bash cloud_user
USER cloud_user
RUN mkdir -p ~/new-dir2
