ARG VARIANT="bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/cpp:0-${VARIANT}

# [Optional] Install CMake version different from what base image has already installed. 
# CMake reinstall choices: none, 3.21.5, 3.22.2, or versions from https://cmake.org/download/
ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="none"

# Optionally install the cmake for vcpkg
COPY ./reinstall-cmake.sh /tmp/
RUN if [ "${REINSTALL_CMAKE_VERSION_FROM_SOURCE}" != "none" ]; then \
    chmod +x /tmp/reinstall-cmake.sh && /tmp/reinstall-cmake.sh ${REINSTALL_CMAKE_VERSION_FROM_SOURCE}; \
    fi \
    && rm -f /tmp/reinstall-cmake.sh

# install imagemagick
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    imagemagick \
    libjpeg-dev \
    libpng++-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# build & install opencv
RUN git clone https://github.com/opencv/opencv.git --depth 1 -b 4.5.5 /tmp/opencv 
COPY build_opencv.sh /tmp/opencv/build_opencv.sh
RUN cd /tmp/opencv \
    && bash ./build_opencv.sh \
    && cd build \
    && make -j4 \
    && make install \
    && rm -rf /tmp/opencv

ENV LD_LIBRARY_PATH /usr/local/lib:${LD_LIBRARY_PATH}
