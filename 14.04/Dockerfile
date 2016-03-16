FROM ubuntu:14.04
MAINTAINER Li Da <lidaemail@qq.com>
# 修改apt-get为阿里镜像源
RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/' /etc/apt/sources.list

RUN apt-get update && apt-get install -y --no-install-recommends \
        openssh-server \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/run/sshd /var/log/supervisor \
    && sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config \
	&& sed -i 's/^PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ONBUILD COPY id_rsa.pub /root/.ssh/authorized_keys
ONBUILD RUN chmod 700 /root/.ssh \
        && chmod 600 /root/.ssh/authorized_keys

# supervisor configuration

RUN echo 'root:root' | chpasswd

EXPOSE 22

CMD ["/etc/init.d/sshd","start"]