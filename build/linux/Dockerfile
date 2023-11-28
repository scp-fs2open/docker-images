FROM docker.io/library/ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

# zstd is required for better GitHub cache compression support
RUN apt-get -yq update && apt-get -yq upgrade && apt-get -yq install software-properties-common gpg wget bash sudo git curl jq zstd

RUN add-apt-repository -y ppa:ubuntu-toolchain-r/test && add-apt-repository -y ppa:deadsnakes/ppa

# Repository for the Vulkan SDK so that we have the glslc compiler
RUN wget -qO - https://packages.lunarg.com/lunarg-signing-key-pub.asc | apt-key add -
RUN wget -qO /etc/apt/sources.list.d/lunarg-vulkan-1.3.224-bionic.list https://packages.lunarg.com/vulkan/1.3.224/lunarg-vulkan-1.3.224-bionic.list

RUN wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | \
	gpg --dearmor - | \
	tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null && \
	apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"

RUN apt-get -yq update && apt-get -yq install cmake=3.25.2-0kitware1ubuntu18.04.1 \
	ninja-build libopenal-dev libreadline6-dev libpng-dev libjpeg62-dev \
	liblua5.1-0-dev libjansson-dev libsdl2-dev libfreetype6-dev valgrind g++-5 g++-13 vulkan-sdk vulkan-headers vulkan-validationlayers libvulkan1 \
	python3.8

COPY llvm.sh /tmp/llvm.sh
RUN /tmp/llvm.sh 16 && apt-get -yq install clang-tidy-16 libc++-16-dev libc++abi-16-dev

COPY ./ccache-4.8.2-linux-x86_64/ /tmp/ccache/
RUN cd /tmp/ccache && make install

# aqt needs at least python 3.7 so we install something that is "good enough" for that. Ubuntu 18.04 does not have a lot of support anymore unfortunately
COPY get-pip.py /tmp/get-pip.py
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1 && \
	python3 /tmp/get-pip.py && \
	mkdir -p /qt && cd /qt && \
	pip3 -vvv install aqtinstall==3.0.3 && \
	aqt install-qt linux desktop 5.12.12 gcc_64

RUN rm -rf /tmp/*

COPY appimagetool /tools/appimagetool

RUN ln -s /tools/appimagetool/AppRun /usr/bin/appimagetool

ENTRYPOINT [ "/bin/bash" ]
