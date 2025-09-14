# Sử dụng base image là Ubuntu 22.04
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# THAY ĐỔI: Cài tightvncserver thay vì tigervnc
RUN apt-get update && apt-get install -y \
    openssh-server \
    sudo \
    vim \
    xfce4 \
    xfce4-goodies \
    tightvncserver \
    && rm -rf /var/lib/apt/lists/*

# Tạo người dùng như cũ
RUN useradd -m -s /bin/bash user && \
    echo "user:1234" | chpasswd && \
    adduser user sudo

# Cấu hình SSH như cũ
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN sed -i 's/UsePAM yes/UsePAM no/' /etc/ssh/sshd_config
RUN mkdir /var/run/sshd

EXPOSE 22 5901

CMD ["/usr/sbin/sshd", "-D"]