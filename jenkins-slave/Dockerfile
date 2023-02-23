FROM jenkins/inbound-agent

USER root
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        locales \
        sudo \
        vim \
        curl \
        telnet \
        git \
        tmux \
        gcc \
        gnupg \
        python3 \
        python3-pip \
        python3-wheel \
        ansible \
        tox \
        ca-certificates \
        apt-transport-https \
        software-properties-common \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
    && curl -sSL https://download.docker.com/linux/debian/gpg -o /tmp/docker.gpg \
    && apt-key add /tmp/docker.gpg \
    && rm /tmp/docker.gpg \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
    && curl -sSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key -o /tmp/node.gpg \
    && apt-key add /tmp/node.gpg \
    && rm /tmp/node.gpg \
    && echo "deb https://deb.nodesource.com/node_16.x $(lsb_release -cs) main" > /etc/apt/sources.list.d/nodesource.list \
    && echo "deb-src https://deb.nodesource.com/node_16.x $(lsb_release -cs) main" >> /etc/apt/sources.list.d/nodesource.list \
    && apt-get update \
    && apt-cache policy docker-ce \
    && apt-get install -y --no-install-recommends docker-ce nodejs \
    && update-alternatives --remove python /usr/bin/python2 \
    && update-alternatives --install /usr/bin/python python /usr/bin/python3 10 \
    && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 \
    && rm -rf /var/lib/apt/lists/*

RUN usermod -aG docker jenkins

RUN pip install --no-cache-dir pbr devpi-client

COPY 010_jenkins-nopasswd /etc/sudoers.d/

USER jenkins
