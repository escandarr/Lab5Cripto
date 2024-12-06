FROM ubuntu:20.10

# Cambiar las fuentes de los repositorios
RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list
RUN sed -i '/^# deb-src/s/^# //' /etc/apt/sources.list

# Actualizar e instalar paquetes necesarios
RUN apt-get update && \
    apt-get install -y openssh-server openssh-client dpkg-dev nano autoconf automake libtool zlib1g-dev libssl-dev && \
    apt-get install -y --fix-missing && \
    apt-get source openssh-client

# Descargar y descomprimir el código fuente de OpenSSH/ aca puedo cambiar la version
RUN wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-8.9p1.tar.gz && \
    tar -xvzf openssh-8.9p1.tar.gz && \
    cd openssh-8.9p1 && \
    sed -i 's/#define SSH_VERSION\t"OpenSSH_.*"/#define SSH_VERSION\t"OpenSSH_?"/' version.h && \ 
    ./configure && \
    make && \
    make install

# Actualizar el cliente SSH predeterminado
RUN ln -sf /usr/local/bin/ssh /usr/bin/ssh

# Configurar el entorno SSH
RUN mkdir /var/run/sshd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Limpiar archivos innecesarios
RUN rm -rf openssh-8.9p1.tar.gz openssh-8.9p1

# Exponer el puerto SSH
EXPOSE 22

# Comando predeterminado para iniciar el servidor SSH
CMD ["/usr/sbin/sshd", "-D"]
