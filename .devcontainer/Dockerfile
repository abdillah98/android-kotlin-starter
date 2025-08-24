FROM mcr.microsoft.com/devcontainers/java:17

# Install basic tools
RUN apt-get update && apt-get install -y wget unzip curl zip gradle

# -------------------------
# Install Kotlin
# -------------------------
RUN wget https://github.com/JetBrains/kotlin/releases/download/v1.9.24/kotlin-compiler-1.9.24.zip -O kotlin.zip \
    && unzip kotlin.zip -d /usr/local \
    && rm kotlin.zip
ENV PATH=$PATH:/usr/local/kotlinc/bin

# -------------------------
# Install Android SDK (Command Line Tools)
# -------------------------
ENV ANDROID_SDK_ROOT=/home/vscode/android-sdk
ENV PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools

RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools \
    && wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip -O cmdtools.zip \
    && unzip cmdtools.zip -d $ANDROID_SDK_ROOT/cmdline-tools \
    && mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest \
    && rm cmdtools.zip

# Accept licenses and install Android platform tools + build tools
RUN yes | sdkmanager --licenses \
    && sdkmanager "platform-tools" "platforms;android-35" "build-tools;35.0.0"

# -------------------------
# Upgrade Gradle Wrapper (default Gradle too old for Android)
# -------------------------
WORKDIR /workspace
RUN gradle wrapper --gradle-version 8.7 --distribution-type all
