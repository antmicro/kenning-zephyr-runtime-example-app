FROM debian:bookworm

ENV INST 'apt-get install -y --no-install-recommends'
ENV PIPINST 'python3 -m pip install --no-cache-dir --upgrade'

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && $INST \
    ccache \
    cmake \
    curl \
    device-tree-compiler \
    dfu-util \
    file \
    fonts-lato \
    gcc \
    gcc-multilib \
    git \
    g++-multilib \
    jq \
    libdbus-1-3 \
    libfontconfig \
    libgl1 \
    libgssapi-krb5-2 \
    libhidapi-hidraw0 \
    libhidapi-libusb0 \
    libmagic1 \
    libncurses5 \
    libsdl2-dev \
    libusb-0.1-4 \
    libxcb1 \
    libxcb-glx0 \
    libxcb-icccm4 \
    libxcb-image0 \
    libxcb-keysyms1 \
    libxcb-randr0 \
    libxcb-render0 \
    libxcb-render-util0 \
    libxcb-shape0 \
    libxcb-shm0 \
    libxcb-sync1 \
    libxcb-util1 \
    libxcb-xfixes0 \
    libxcb-xinerama0 \
    libxcb-xkb1 \
    libxkbcommon0 \
    libxkbcommon-x11-0 \
    make \
    ninja-build \
    patch \
    picocom \
    python3-dev \
    python3-pip \
    python3-setuptools \
    python3-tk \
    python3-venv \
    python3-wheel \
    wget \
    xxd \
    xz-utils \
    && apt-get autoremove && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN rm /usr/lib/python3.11/EXTERNALLY-MANAGED

RUN wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && dpkg -i packages-microsoft-prod.deb && rm packages-microsoft-prod.deb

RUN apt-get update && $INST dotnet-sdk-9.0 && apt-get autoremove && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN wget -O /tmp/renode-portable-dotnet.tar.gz https://builds.renode.io/renode-latest.linux-portable-dotnet.tar.gz && \
    mkdir -p /opt/renode/ && \
    tar -C /opt/renode --strip-components=1 -xzvf /tmp/renode-portable-dotnet.tar.gz && rm /tmp/renode-portable-dotnet.tar.gz

ENV PYRENODE_RUNTIME=coreclr
ENV PYRENODE_BIN=/opt/renode/renode

RUN $PIPINST \
    pip \
    setuptools \
    west \
    pyelftools

RUN wget -O /tmp/prepare_zephyr_env.sh https://raw.githubusercontent.com/antmicro/kenning-zephyr-runtime/refs/heads/stable/scripts/prepare_zephyr_env.sh && \
    chmod +x /tmp/prepare_zephyr_env.sh && \
    ZEPHYR_SDK_ONLY=1 ZEPHYR_SDK_PATH=/opt/zephyr-sdk /tmp/prepare_zephyr_env.sh && \
    rm /tmp/prepare_zephyr_env.sh

RUN wget -O /tmp/requirements.txt https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/refs/heads/main/scripts/requirements-base.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/refs/heads/main/scripts/requirements-build-test.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/refs/heads/main/scripts/requirements-run-test.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/antmicro/kenning-zephyr-runtime/refs/heads/main/requirements.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    rm /tmp/requirements.txt

ENV ZEPHYR_SDK_INSTALL_DIR /opt/zephyr-sdk
ENV ZEPHYR_SDK_PATH /opt/zephyr-sdk

RUN $PIPINST \
    "kenning[anomaly_detection,iree,tvm,tensorflow,tflite,torch,reports,renode,uart,auto_pytorch] @ git+https://github.com/antmicro/kenning.git"
