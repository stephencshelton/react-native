FROM ubuntu:22.04

# Android SDK versions
ARG ANDROID_SDK_VERSION=9477386_latest
ARG ANDROID_BUILD_TOOLS_VERSION=33.0.0
ARG ANDROID_PLATFORM_VERSION=33

# Update package lists and install necessary dependencies
RUN apt-get update && apt-get install -y curl apt-transport-https gnupg gnupg2 wget unzip libpulse0 libxcursor1 libxrandr2 libxinerama1 libxdamage1 libgl1-mesa-glx libgtk-3-0 qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils

# Add the Yarn repository to your sources list
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Update package lists again and install Yarn
RUN apt-get update && apt-get install -y yarn

# Install OpenJDK 11
RUN apt-get update && \
    apt-get install -y openjdk-11-jdk

# Check Java version
RUN java -version

# Setup Android emulator
RUN mkdir /usr/local/android-sdk
ENV ANDROID_HOME /usr/local/android-sdk
ENV PATH ${PATH}:${ANDROID_HOME}/cmdline-tools/tools/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator
RUN wget https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_VERSION}.zip && \
    unzip commandlinetools-linux-${ANDROID_SDK_VERSION}.zip -d ${ANDROID_HOME}/cmdline-tools && \
    mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/tools && \
    rm commandlinetools-linux-${ANDROID_SDK_VERSION}.zip

# Install Android build tools, platform tools, emulator, and system image
RUN yes | sdkmanager --licenses && \
    sdkmanager "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" "platforms;android-${ANDROID_PLATFORM_VERSION}" "platform-tools" "emulator" "system-images;android-${ANDROID_PLATFORM_VERSION};google_apis;x86_64"

# Create Android Virtual Device (AVD) for the emulator
ENV ANDROID_AVD_HOME /root/.android/avd
RUN echo "no" | avdmanager create avd -n react-native -k "system-images;android-${ANDROID_PLATFORM_VERSION};google_apis;x86_64" -f