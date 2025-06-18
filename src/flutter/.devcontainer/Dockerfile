FROM mcr.microsoft.com/devcontainers/base:debian

ARG USER=vscode
ARG FLUTTER_VERSION=stable

# Install linux dependencies
RUN apt-get update &&\
    apt-get install -y clang cmake ninja-build pkg-config libgtk-3-dev &&\
    apt dist-upgrade -y

# Get latest Android SDK
ENV ANDROID_HOME=/opt/android-sdk-linux
ENV ANDROID_SDK_ROOT=$ANDROID_HOME
COPY --from=mobiledevops/android-sdk-image $ANDROID_HOME $ANDROID_HOME
RUN mkdir $ANDROID_HOME/cmdline-tools/latest &&\
    (cd $ANDROID_HOME/cmdline-tools && ls | grep -v latest | xargs mv -t latest)
ENV PATH="${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:${PATH}"

# Get latest Java SDK
ENV JAVA_HOME=/opt/java/openjdk
COPY --from=eclipse-temurin:latest $JAVA_HOME $JAVA_HOME
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# Get latest Flutter
ENV FLUTTER_HOME=/usr/bin/flutter
RUN mkdir -p $FLUTTER_HOME &&\
    git clone -b stable https://github.com/flutter/flutter $FLUTTER_HOME
ENV PATH="${FLUTTER_HOME}/bin:${PATH}"
RUN chmod +x $FLUTTER_HOME/bin/flutter &&\
    git config --global --add safe.directory '*' &&\
    flutter precache

# Install flutter bash completion
RUN chown 1000:1000 $FLUTTER_HOME -R &&\
    chown 1000:1000 $JAVA_HOME -R &&\
    chown 1000:1000 $ANDROID_HOME -R
USER ${USER}

# Source flutter bash completion
RUN echo 'source <(flutter bash-completion)' >> ~/.bashrc &&\
    sdkmanager --version &&\
    java --version &&\
    flutter doctor
