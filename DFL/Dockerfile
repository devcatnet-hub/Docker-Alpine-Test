FROM alpine:latest

LABEL Maintainer="CatDEV <dev.catnet@gmail.com>"

### ENV 
ENV TZ=America/Guatemala

### COPY
COPY ./PRC/motd /etc/motd
COPY ./PRC/profile /etc/profile
COPY ./PRC/authorized_keys /root/.ssh/authorized_keys
COPY SCP /SCP

### [ RUN ]
### [!]: Replace \ + && with ;\
RUN set -eux ;\
### Add Edge, Edge Community and Testing Repo for Optinal Use
# [NOTA]: El Test se usa solo para "Ensuciar", es para no usar la oficial o bien usar diferentes versiones y ver cual funciona mejor
#         ademas que tiene habilitado los repositorios Edge
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories ;\
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories ;\
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories ;\
### Install Basic Packages
    apk --no-cache add \
### Bash
        bash \
### SSH
        openssh \
### Basic Packages
# [NOTA]: ncurses fue desactivado por no ser binario nesesario.
        wget \
        curl \
        nano \
        # ncurses \
        dumb-init \
        tzdata ;\
### Set-up SHH Server
    ssh-keygen -A ;\
    sed -i s/#PermitRootLogin.*/PermitRootLogin\ yes/ /etc/ssh/sshd_config ;\
    echo "root:Shaka.66" | chpasswd ;\
### Setting bash as default in root
    sed -i s/ash/bash/g /etc/passwd ;\
### Setting the Timezone
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone ;\
### Set authorized_keys 
    chmod 700 /root/.ssh ;\
    chmod 600 /root/.ssh/authorized_keys ;\
### Set Script Dir
    chmod +x /SCP/* ;\
### Set COM Dir
    mkdir /COM ;\
    chmod -R 777 /COM ;\
### Clean [!]
    /SCP/clean.sh

### [ EXPOSE ]
EXPOSE 22

### [ ENTRYPOINT ]
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

### [ CMD ]
CMD ["/SCP/entrypoint.sh"]