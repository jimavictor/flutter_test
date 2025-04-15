FROM dart

# Install system dependencies
RUN apt-get update && apt-get install -y \
  curl \
  git \
  unzip \
  xz-utils \
  zip \
  libglu1-mesa \
  libncurses5 \
  libstdc++6 \
  lib32z1 \
  libbz2-1.0 \
  lib32stdc++6 \
  libc6-i386 \
  lib32gcc1 \
  lib32ncurses6 \
  lib32tinfo6 \
  libx11-6 \
  qemu-kvm \
  && apt-get clean

# Install Flutter
RUN git clone https://github.com/flutter/flutter.git -b stable --depth 1 /flutter
ENV PATH="/flutter/bin:/flutter/bin/cache/dart-sdk/bin:$PATH"

# Accept Android licenses early
RUN flutter doctor --android-licenses || true

# Install latest CMake
RUN curl -L https://github.com/Kitware/CMake/releases/latest/download/cmake-3.29.3-linux-x86_64.sh -o cmake.sh && \
    chmod +x cmake.sh && \
    ./cmake.sh --skip-license --prefix=/usr/local && \
    rm cmake.sh

# Install Android Command Line Tools
ENV ANDROID_SDK_ROOT=/opt/android-sdk
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
    curl -o sdk.zip https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip && \
    unzip sdk.zip -d $ANDROID_SDK_ROOT/cmdline-tools && \
    mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
    rm sdk.zip

ENV PATH="$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH"

# Install SDK components (including emulator)
RUN yes | sdkmanager --licenses && \
    sdkmanager \
      "platform-tools" \
      "platforms;android-33" \
      "build-tools;33.0.2" \
      "emulator" \
      "system-images;android-33;google_apis;x86_64"

# Set Flutter config for Android
RUN flutter config --android-sdk $ANDROID_SDK_ROOT

# Pre-cache Flutter dependencies
RUN flutter doctor

