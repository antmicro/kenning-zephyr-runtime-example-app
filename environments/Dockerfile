FROM ghcr.io/antmicro/kenning-zephyr-runtime:latest


RUN wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.17.4/zephyr-sdk-0.17.4_linux-x86_64.tar.xz && \
    wget -O - https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.17.4/sha256.sum | shasum --check --ignore-missing && \
    mkdir /opt/zephyr-sdk && \
    tar xvf zephyr-sdk-0.17.4_linux-x86_64.tar.xz -C /opt/zephyr-sdk && \
    rm zephyr-sdk-0.17.4_linux-x86_64.tar.xz

RUN wget -O /tmp/requirements.txt https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/refs/heads/main/scripts/requirements-base.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/refs/heads/main/scripts/requirements-build-test.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/refs/heads/main/scripts/requirements-run-test.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/antmicro/kenning-zephyr-runtime/refs/heads/main/requirements.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/antmicro/zephelin/refs/heads/main/requirements.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    rm /tmp/requirements.txt

ENV ZEPHYR_SDK_INSTALL_DIR /opt/zephyr-sdk
ENV ZEPHYR_SDK_PATH /opt/zephyr-sdk

RUN $PIPINST \
    "kenning[anomaly_detection,iree,tvm,tensorflow,tflite,torch,reports,renode,uart,auto_pytorch] @ git+https://github.com/antmicro/kenning.git"
