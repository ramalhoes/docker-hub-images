FROM ubuntu:19.04

# copy in our source code last, as it changes the most
RUN mkdir -p /opt/app
WORKDIR /opt/app
COPY . /opt/app

# Update is used to resynchronize the package index files from their sources. An update should always be performed before an install.
RUN apt-get update \
    && apt-get install -y wget \
    xvfb \
    firefox \
    nodejs \
    npm

# Update package a npm for latest
RUN  npm install npm@latest -g \
    && npm install -g protractor \
    && npm cache clean --force \
    && webdriver-manager update