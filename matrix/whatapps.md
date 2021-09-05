# Running whatsapp in a LXC container

## Add ssh for direct access

```bash
apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    dbus dbus-x11 \
    nano \
    libxcomposite-dev libxcursor-dev libpulse0 \
    openjdk-11-jdk \
    unzip \
    wget \
    x11-apps xauth

# For ANDROID_CLT_VERSION version check https://developer.android.com/studio #Command line tools only
# For ANDROID_SDK_VERSION check platform version : https://developer.android.com/studio/releases/platforms

tee --append /etc/environment > /dev/null <<EOT
ANDROID_CLT_VERSION=6858069
ANDROID_HOME="/opt/android-sdk"
ANDROID_SDK_VERSION=30
QTWEBENGINE_DISABLE_SANDBOX=1
EOT

# Test x11 forwarding with 
/usr/bin/xclock

source /etc/environment

wget -O /tmp/clt.zip -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CLT_VERSION}_latest.zip \
    && unzip /tmp/clt.zip -d ${ANDROID_HOME}/cmdline-tools/${ANDROID_CLT_VERSION} \
    && ln -s ${ANDROID_HOME}/cmdline-tools/${ANDROID_CLT_VERSION}/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest \
    && rm /tmp/clt.zip

/opt/android-sdk/accept_license.sh $ANDROID_HOME

"$ANDROID_HOME/cmdline-tools/latest/bin/sdkmanager" "platform-tools" "platforms;android-${ANDROID_SDK_VERSION}" "system-images;android-${ANDROID_SDK_VERSION};google_apis_playstore;x86_64"

"$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager" create avd -n whatsapp -d 25 -k "system-images;android-${ANDROID_SDK_VERSION};google_apis_playstore;x86_64"

"$ANDROID_HOME/emulator/emulator" -show-kernel -no-boot-anim -no-audio -avd whatsapp
```

FROM debian:buster

# gcc for cgo
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    g++ \
    gcc \
    git \
    gnupg \
    libc6-dev lib32gcc1 lib32z1 \
    make \
    openjdk-11-jdk \
    pkg-config \
    python3 python3-pip python3-openssl python3-setuptools python3-venv python3-wheel \
    software-properties-common \
    unzip \
    wget 

# Install Kotlin compiler
ARG KOTLIN_VERSION=1.3.71
ENV KOTLIN_HOME /opt/kotlinc
RUN wget -O /tmp/kotlinc.zip -q https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip \
  && unzip /tmp/kotlinc.zip -d /opt \
  && rm /tmp/kotlinc.zip

# Install Android SDK & NDK
ARG ANDROID_CLT_VERSION=6858069
ARG ANDROID_SDK_VERSION=29
ARG ANDROID_NDK_VERSION=21.2.6472646
ARG ANDROID_BUILD_TOOLS_VERSION=28.0.3

ENV ANDROID_HOME /opt/android-sdk
ENV ANDROID_NDK_HOME /opt/android-sdk/ndk/${ANDROID_NDK_VERSION}/
ADD docker/accept_license.sh /opt/

RUN mkdir -p ${ANDROID_HOME}/cmdline-tools \
    && wget -O /tmp/clt.zip -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CLT_VERSION}_latest.zip \
    && unzip /tmp/clt.zip -d ${ANDROID_HOME}/cmdline-tools \
    && rm /tmp/clt.zip \
    && chmod +x /opt/accept_license.sh \
    && /opt/accept_license.sh $ANDROID_HOME \
    && "$ANDROID_HOME/cmdline-tools/tools/bin/sdkmanager" "platform-tools" "platforms;android-${ANDROID_SDK_VERSION}" "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" "ndk;${ANDROID_NDK_VERSION}"

# Install Gradle
ARG GRADLE_VERSION=6.3
ARG GRADLE_DIST=bin
ENV GRADLE_HOME /opt/gradle
RUN wget -q https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-${GRADLE_DIST}.zip -O /tmp/gradle.zip \
    && unzip /tmp/gradle.zip -d /opt/ \
    && mv "/opt/gradle-$GRADLE_VERSION" "$GRADLE_HOME" \
    && rm /tmp/gradle.zip

# Install Golang
ENV GOLANG_VERSION 1.14.2
ENV GOPATH /go
RUN set -eux \
  && goRelArch='linux-amd64' \
  && goRelSha256='6272d6e940ecb71ea5636ddb5fab3933e087c1356173c61f4a803895e947ebb3' \
  && wget -O go.tgz "https://golang.org/dl/go${GOLANG_VERSION}.${goRelArch}.tar.gz" \
  && echo "${goRelSha256} *go.tgz" | sha256sum -c - \
  && tar -C /usr/local -xzf go.tgz \
  && rm go.tgz \
  && export PATH="/usr/local/go/bin:$PATH" \
  && go version \
  && mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH" \
  && GO111MODULE=off go get golang.org/x/mobile/cmd/gomobile \
  && "$GOPATH/bin/gomobile" init -v

ENV PATH ${KOTLIN_HOME}/bin:${GRADLE_HOME}/bin:$GOPATH/bin:/usr/local/go/bin:$PATH
WORKDIR $GOPATH