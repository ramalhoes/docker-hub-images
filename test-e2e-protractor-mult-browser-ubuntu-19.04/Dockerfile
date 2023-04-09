FROM ubuntu:19.04

RUN mkdir -p /opt/app
WORKDIR /opt/app

# Update is used to resynchronize the package index files from their sources. An update should always be performed before an install.
RUN apt-get update \
    && apt-get install -y wget \
    xvfb \
    firefox \
    nodejs \
    npm

# Update package a npm for latest
RUN  npm install npm@latest -g

# Install dependences and install protractor global and clear cache from npm and update webdriver-manager
RUN npm install -g protractor \
    && npm cache clean --force \
    && webdriver-manager update

# Latest Google Chrome installation package
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN dpkg -i google-chrome-stable_current_amd64.deb; apt-get -fy install