FROM ghcr.io/antmicro/kenning-zephyr-runtime:latest


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
    wget -O /tmp/requirements.txt https://raw.githubusercontent.com/antmicro/zephelin/refs/heads/main/requirements.txt && \
    $PIPINST -r /tmp/requirements.txt && \
    rm /tmp/requirements.txt

ENV ZEPHYR_SDK_INSTALL_DIR /opt/zephyr-sdk
ENV ZEPHYR_SDK_PATH /opt/zephyr-sdk

RUN $PIPINST \
    "kenning[anomaly_detection,iree,tvm,tensorflow,tflite,torch,reports,renode,uart,auto_pytorch] @ git+https://github.com/antmicro/kenning.git"

# TODO: Change remote name
RUN git clone --recurse-submodules https://github.com/pedro-zawadniak-adi/ai8x-training.git && \
    python3 -m venv ai8x-training/.venv && \
    cd ai8x-training && \
    git checkout update-dependencies && \
    .venv/bin/pip install --no-cache --upgrade pip && \
    .venv/bin/pip install --no-cache -r requirements.txt && \
    cd -

RUN git clone https://github.com/analogdevicesinc/ai8x-synthesis.git && \
    python3 -m venv ai8x-synthesis/.venv && \
    ai8x-synthesis/.venv/bin/pip install --no-cache --upgrade pip && \
    ai8x-synthesis/.venv/bin/pip install --no-cache -r ai8x-synthesis/requirements.txt

ENV AI8X_TRAINING_PATH=/ai8x-training
ENV AI8X_SYNTHESIS_PATH=/ai8x-synthesis

# TODO: Remove this:
RUN wget -O /tmp/MaximMicrosSDK_linux.run https://github.com/analogdevicesinc/msdk/releases/download/v2024_10/MaximMicrosSDK_linux.run && \
    chmod +x /tmp/MaximMicrosSDK_linux.run && \
    /tmp/MaximMicrosSDK_linux.run install --accept-messages --accept-licenses --confirm-command && \
    rm /tmp/MaximMicrosSDK_linux.run

ENV PATH "$PATH:/root/MaximSDK/Tools/OpenOCD/"
ENV MAXIM_SDK /root/MaximSDK
