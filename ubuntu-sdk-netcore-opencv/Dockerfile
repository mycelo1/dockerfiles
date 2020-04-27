FROM mcr.microsoft.com/dotnet/core/sdk:3.1-bionic AS base

ENV OPENCV_VERSION="4.3.0"
ENV OPENCV_SHARP_VERSION="4.3.0.20200421"
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y \
	build-essential \
	checkinstall \
	cmake \
	doxygen \
	gfortran \
	git \
	libatlas-base-dev \
	libavcodec-dev \
	libavformat-dev \
	libavresample-dev \
	libdc1394-22-dev \
	libeigen3-dev \
	libfaac-dev \
	libgflags-dev \
	libgoogle-glog-dev \
	libgphoto2-dev \
	libgstreamer-plugins-base1.0-dev \
	libgstreamer1.0-dev \
	libgtk2.0-dev \
	libhdf5-dev \
	libjpeg-dev \
	libjpeg8-dev \
	libmp3lame-dev \
	libopencore-amrnb-dev \
	libopencore-amrwb-dev \
	libpng-dev \
	libprotobuf-dev \
	libswscale-dev \
	libtbb-dev \
	libtbb2 \
	libtheora-dev \
	libtiff-dev \
	libv4l-dev \
	libvorbis-dev \
	libxine2-dev \
	libxvidcore-dev \
	pkg-config \
	protobuf-compiler \
	python3-opencv \
	qt5-default \
	unzip \
	v4l-utils \
	wget \
	x264 \
	yasm

WORKDIR /opt/opencv
RUN wget -q https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip --output-document=opencv-${OPENCV_VERSION}.zip
RUN unzip -q opencv-${OPENCV_VERSION}.zip
RUN wget -q https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip --output-document=opencv_contrib-${OPENCV_VERSION}.zip
RUN unzip -q opencv_contrib-${OPENCV_VERSION}.zip

WORKDIR /opt/opencv/build
RUN cmake /opt/opencv/opencv-${OPENCV_VERSION} -DCMAKE_BUILD_TYPE=Release \
	-DOPENCV_EXTRA_MODULES_PATH=/opt/opencv/opencv_contrib-${OPENCV_VERSION}/modules && \
	make -j$(grep -c ^processor /proc/cpuinfo) && \
	make install -j8 && \
	ldconfig

WORKDIR /opt/opencvsharp
RUN git clone https://github.com/shimat/opencvsharp.git .

WORKDIR /opt/opencvsharp/build
RUN cmake /opt/opencvsharp/src -DCMAKE_INSTALL_PREFIX=/opt/opencv && \
	make -j$(grep -c ^processor /proc/cpuinfo) && \
	make install && \
	ldconfig
