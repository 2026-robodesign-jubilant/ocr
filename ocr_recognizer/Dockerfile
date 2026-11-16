# ocr_recognizer（ocr_node）を動かすイメージ
# ビルドと起動はリポジトリのルートの compose.yaml から行う（docker compose up --build ocr）

FROM ros:jazzy-ros-base

SHELL ["/bin/bash", "-c"]

# ROS の部品・OpenCV・音を鳴らす道具
#   alsa-utils        : aplay（ノードが音声を鳴らすのに使う）
#   libasound2-plugins: aplay の音を、ホストの PulseAudio / PipeWire に流すための部品
#   image-publisher   : カメラなしで試すとき、写真をトピックに流す道具
RUN apt-get update && apt-get install -y --no-install-recommends \
        ros-jazzy-cv-bridge \
        ros-jazzy-image-publisher \
        python3-opencv \
        python3-requests \
        python3-pip \
        alsa-utils \
        libasound2-plugins \
    && rm -rf /var/lib/apt/lists/*

# Claude の SDK。コンテナの中なので仮想環境は作らず、そのまま入れる
#   pydantic は 2 以上、brotli は 1.2 以上が必要（apt の古い版だと動かない。README のハマりどころ参照）
RUN pip3 install --no-cache-dir --break-system-packages \
        anthropic "pydantic>=2" "brotli>=1.2"

# aplay の出力先を PulseAudio にする（compose.yaml でホストの音声サーバーにつないでいる）
RUN printf 'pcm.!default pulse\nctl.!default pulse\n' > /etc/asound.conf

# パッケージをコピーしてビルド（コピーしないものは .dockerignore に書いてある）
WORKDIR /ws
COPY . src/ocr_recognizer
RUN source /opt/ros/jazzy/setup.bash && colcon build --packages-select ocr_recognizer

CMD ["bash", "-c", "source /ws/install/setup.bash && exec ros2 run ocr_recognizer ocr_node"]
