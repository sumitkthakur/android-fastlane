FROM openjdk:8-slim
LABEL maintainer Divick K.<divick@gorapid.io>

ENV SDK_TOOLS="4333796" \
    BUILD_TOOLS="28.0.2" \
    TARGET_SDK="28" \
    ANDROID_HOME="/opt/sdk" \
    RUBY_VERSION="2.4.2" \
    PATH=/root/.rbenv/shims:/root/.rbenv/bin:/root/.rbenv/plugins/ruby-build/bin:$PATH

# Install required dependencies
RUN apt-get update && \
    apt-get install -y \
        ca-certificates \
        libssl-dev \
        libreadline-dev \
        zlib1g-dev \
        gcc \
        g++ \
        make \
        wget \
        git \
    && rm -rf /var/cache/apt/* \
    && rm -rf /var/lib/apt/lists/*

# Download and extract Android Tools
RUN wget http://dl.google.com/android/repository/sdk-tools-linux-${SDK_TOOLS}.zip -O /tmp/tools.zip && \
    mkdir -p ${ANDROID_HOME} && \
    unzip /tmp/tools.zip -d ${ANDROID_HOME} && \
    rm -v /tmp/tools.zip

# Install SDK Packages
RUN mkdir -p /root/.android/ && touch /root/.android/repositories.cfg && \
    yes | ${ANDROID_HOME}/tools/bin/sdkmanager "--licenses" && \
    ${ANDROID_HOME}/tools/bin/sdkmanager "--update" && \
    ${ANDROID_HOME}/tools/bin/sdkmanager "build-tools;${BUILD_TOOLS}" "platform-tools" "platforms;android-${TARGET_SDK}" "extras;android;m2repository" "extras;google;google_play_services" "extras;google;m2repository" "emulator"

# Install ruby via rbenv and ruby-build
RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv \
    && git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build \
    && echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh \
    && echo 'eval "$(rbenv init -)"' >> /root/.bashrc \
    && mkdir -p "$(rbenv root)"/plugins \
    && rbenv install ${RUBY_VERSION} \
    && rbenv global ${RUBY_VERSION}
