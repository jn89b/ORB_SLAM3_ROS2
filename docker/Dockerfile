FROM osrf/ros:humble-desktop-full-jammy as base
RUN apt-get update

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y gnupg2 curl lsb-core vim wget python3-pip libpng16-16 libjpeg-turbo8 libtiff5

RUN apt-get install -y \
    # Base tools
    cmake \
    build-essential \
    git \
    unzip \
    pkg-config \
    python3-dev \
    # OpenCV dependencies
    python3-numpy \
    # Pangolin dependencies
    libgl1-mesa-dev \
    libglew-dev \
    libpython3-dev \
    libeigen3-dev \
    apt-transport-https \
    ca-certificates\
    software-properties-common

RUN apt update


# Build OpenCV
RUN apt-get install -y python3-dev python3-numpy python2-dev
RUN apt-get install -y libavcodec-dev libavformat-dev libswscale-dev
RUN apt-get install -y libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
RUN apt-get install -y libgtk-3-dev

RUN cd /tmp && git clone https://github.com/opencv/opencv.git && \
    cd opencv && \
    git checkout 4.4.0 && mkdir build && cd build && \
    cmake -D CMAKE_BUILD_TYPE=Release -D BUILD_EXAMPLES=OFF  -D BUILD_DOCS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_TESTS=OFF -D CMAKE_INSTALL_PREFIX=/usr/local .. && \
    make -j8 && make install && \
    cd / && rm -rf /tmp/opencv

# Build Pangolin
RUN cd /tmp && git clone https://github.com/stevenlovegrove/Pangolin && \
    cd Pangolin && git checkout v0.9.1 && mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS=-std=c++14 -DCMAKE_INSTALL_PREFIX=/usr/local .. && \
    make -j8 && make install && \
    cd / && rm -rf /tmp/Pangolin
COPY ./docker/entrypoint.sh /
ENTRYPOINT [ "/entrypoint.sh" ]

## Development image
## Development image
FROM base AS develop

SHELL ["/bin/bash", "-c"]

#Create an overlay workspace
RUN mkdir -p /develop_ws/src \
    && echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc 

WORKDIR /develop_ws/src

RUN cd /develop_ws/ \
    && colcon build --symlink-install \
    && source install/setup.bash  \
    && apt-get update && apt-get install -y tmux 

 # Set up the entrypoint
COPY ./docker/entrypoint.sh ./useful_shell_scripts/run_mavros_router.sh /
COPY ./useful_shell_scripts/run_live.sh /

ENTRYPOINT [ "/entrypoint.sh" ]