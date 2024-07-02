FROM ubuntu:18.10

# Cambiar las fuentes de los repositorios
RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i '/^# deb-src/s/^# //' /etc/apt/sources.list

# Actualizar e instalar paquetes necesarios
RUN apt-get update && \
    apt-get install -y openssh-server openssh-client dpkg-dev nano autoconf automake libtool zlib1g-dev libssl-dev && \
    apt-get install -y --fix-missing && \
    apt-get source openssh-client

# Configurar el entorno SSH
RUN mkdir /var/run/sshd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Exponer el puerto SSH
EXPOSE 22

# Comando para iniciar SSH
CMD ["/usr/sbin/sshd", "-D"]
