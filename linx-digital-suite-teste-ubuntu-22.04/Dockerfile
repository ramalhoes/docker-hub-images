FROM ubuntu:22.04

ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'
ENV DEBIAN_FRONTEND=noninteractive

# copy in our source code last, as it changes the most
RUN mkdir -p /opt/app
WORKDIR /opt/app
COPY . /opt/app

RUN apt-get update \
    && apt-get -y install tzdata \
    && apt-get install -y wget \
    gnupg2 lsb-release \
    xvfb \
    firefox \
    && apt-get install git -y \
    && apt-get install -y --no-install-recommends tzdata curl ca-certificates fontconfig locales \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/*

# Latest Google Chrome installation package
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list' \
    && apt-get update \
    && apt install google-chrome-stable -y

RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
    apt-key fingerprint 1655A0AB68576280 && \
    export VERSION=node_19.x && \
    export DISTRO="$(lsb_release -s -c)" && \
    echo "deb https://deb.nodesource.com/$VERSION $DISTRO main" | tee /etc/apt/sources.list.d/nodesource.list && \
    echo "deb-src https://deb.nodesource.com/$VERSION $DISTRO main" | tee -a /etc/apt/sources.list.d/nodesource.list && \
    apt-get update && apt-get install -y nodejs && \
    node -v && npm -v

RUN apt-get update && apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:mozillateam/firefox-next
RUN apt-get update && apt-get install -y firefox

RUN apt-get install -y ffmpeg

# Update package a npm for latest
# npm install -g npm-check-updates \
# npm install npm@latest -g

# Update package a npm for latest
RUN npm install -g npm@9.1.2 \
    && npm install -g npm-check-updates

# Install dependences and install webdriverio
RUN npm install webdriverio

# Install dependences and install K6
RUN apt-get install jq -y
RUN apt-key adv --no-tty --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
RUN echo "deb https://dl.k6.io/deb stable main" | tee /etc/apt/sources.list.d/k6.list
RUN apt-get update
RUN apt-get install k6

# Install dependences and install protractor global and clear cache from npm and update webdriver-manager
RUN npm install -g protractor \
    && npm cache clean --force \
    && webdriver-manager Update

# - https://blog.locaweb.com.br/temas/codigo-aberto/como-enviar-suas-imagens-docker-para-o-dockerhub/