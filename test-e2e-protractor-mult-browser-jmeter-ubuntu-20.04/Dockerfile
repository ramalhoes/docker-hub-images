FROM ubuntu:20.04
  
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'
ENV DEBIAN_FRONTEND=noninteractive

# copy in our source code last, as it changes the most
RUN mkdir -p /opt/app
WORKDIR /opt/app
COPY . /opt/app

RUN apt-get update \
    && apt-get -y install tzdata \
    && apt-get install -y wget \
    xvfb \
    firefox \
    nodejs \
    npm \
    && apt-get install -y --no-install-recommends tzdata curl ca-certificates fontconfig locales \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/*

# Latest Google Chrome installation package
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list' \
    && apt-get update \
    && apt install google-chrome-stable

# Update package a npm for latest
RUN  npm install npm@latest -g \
    && npm install -g protractor \
    && npm cache clean --force \
    && webdriver-manager update

# Update is used to resynchronize the package index files from their sources. An update should always be performed before an install.
RUN wget http://apache.volia.net//jmeter/binaries/apache-jmeter-5.3.tgz && tar -xf apache-jmeter-5.3.tgz

ENV JAVA_VERSION jdk-11.0.8+10

RUN set -eux; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
       aarch64|arm64) \
         ESUM='fb27ea52ed901c14c9fe8ad2fc10b338b8cf47d6762571be1fe3fb7c426bab7c'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.8%2B10/OpenJDK11U-jdk_aarch64_linux_hotspot_11.0.8_10.tar.gz'; \
         ;; \
       armhf|armv7l) \
         ESUM='d00370967e4657e137cc511e81d6accbfdb08dba91e6268abef8219e735fbfc5'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.8%2B10/OpenJDK11U-jdk_arm_linux_hotspot_11.0.8_10.tar.gz'; \
         ;; \
       ppc64el|ppc64le) \
         ESUM='d206a63cd719b65717f7f20ee3fe49f0b8b2db922986b4811c828db57212699e'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.8%2B10/OpenJDK11U-jdk_ppc64le_linux_hotspot_11.0.8_10.tar.gz'; \
         ;; \
       s390x) \
         ESUM='5619e1437c7cd400169eb7f1c831c2635fdb2776a401147a2fc1841b01f83ed6'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.8%2B10/OpenJDK11U-jdk_s390x_linux_hotspot_11.0.8_10.tar.gz'; \
         ;; \
       amd64|x86_64) \
         ESUM='6e4cead158037cb7747ca47416474d4f408c9126be5b96f9befd532e0a762b47'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.8%2B10/OpenJDK11U-jdk_x64_linux_hotspot_11.0.8_10.tar.gz'; \
         ;; \
       *) \
         echo "Unsupported arch: ${ARCH}"; \
         exit 1; \
         ;; \
    esac; \
    curl -LfsSo /tmp/openjdk.tar.gz ${BINARY_URL}; \
    echo "${ESUM} */tmp/openjdk.tar.gz" | sha256sum -c -; \
    mkdir -p /opt/java/openjdk; \
    cd /opt/java/openjdk; \
    tar -xf /tmp/openjdk.tar.gz --strip-components=1; \
    rm -rf /tmp/openjdk.tar.gz;

ENV JAVA_HOME=/opt/java/openjdk \
    PATH="/opt/java/openjdk/bin:$PATH"