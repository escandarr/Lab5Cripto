FROM ubuntu:22.10

RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i '/^# deb-src/s/^# //' /etc/apt/sources.list

RUN apt-get update && \
    apt-get install -y openssh-server openssh-client

RUN useradd -m prueba && echo "prueba:prueba" | chpasswd

CMD ["/usr/sbin/sshd", "-D"]
