FROM docker.io/library/ubuntu:18.04

RUN apt-get -yq update && apt-get -yq install software-properties-common gpg wget bash sudo git python3-pip curl jq zstd

RUN add-apt-repository -y ppa:ubuntu-toolchain-r/test

# Repository for the Vulkan SDK so that we have the glslc compiler
RUN wget -qO - https://packages.lunarg.com/lunarg-signing-key-pub.asc | apt-key add -
RUN wget -qO /etc/apt/sources.list.d/lunarg-vulkan-1.3.224-bionic.list https://packages.lunarg.com/vulkan/1.3.224/lunarg-vulkan-1.3.224-bionic.list

RUN wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | \
	gpg --dearmor - | \
	tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null && \
	apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"

RUN apt-get -yq update && apt-get -yq install cmake=3.25.2-0kitware1ubuntu18.04.1 \
	ninja-build libopenal-dev libreadline6-dev libpng-dev libjpeg62-dev \
	liblua5.1-0-dev libjansson-dev libsdl2-dev libfreetype6-dev valgrind g++-5 g++-13 vulkan-headers vulkan-validationlayers libvulkan1

COPY llvm.sh /tmp/llvm.sh
RUN /tmp/llvm.sh 16 && apt-get -yq install clang-tidy-16 libc++-16-dev libc++abi-16-dev

COPY ./ccache-4.8.2-linux-x86_64/ /tmp/ccache/
RUN cd /tmp/ccache && make install

RUN rm -rf /tmp

COPY appimagetool /tools/appimagetool

RUN ln -s /tools/appimagetool/AppRun /usr/bin/appimagetool

ENTRYPOINT [ "/bin/bash" ]
