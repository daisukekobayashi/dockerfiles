FROM nvidia/cuda:10.0-cudnn7-devel
MAINTAINER daisukekobayashi <daisuke@daisukekobayashi.com>

ENV DEBIAN_FRONTEND noninteractivea

RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 apt-transport-https ca-certificates \
            git curl cmake build-essential libopencv-dev \
      && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/AlexeyAB/darknet.git && cd darknet \
      && sed -i -e 's/GPU\=0/GPU\=1/g' Makefile \
      && sed -i -e 's/CUDNN\=0/CUDNN\=1/g' Makefile \
      && sed -i -e 's/OPENCV\=0/OPENCV\=1/g' Makefile \
      && make && cp darknet /usr/local/bin && cd .. && rm -rf darknet
