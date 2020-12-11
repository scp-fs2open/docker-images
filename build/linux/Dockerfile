FROM ubuntu:18.04

RUN apt-get -yq update && apt-get -yq install software-properties-common wget bash sudo git

RUN add-apt-repository -y ppa:ubuntu-toolchain-r/test

# Repository for the Vulkan SDK so that we have the glslc compiler
RUN wget -qO - https://packages.lunarg.com/lunarg-signing-key-pub.asc | apt-key add -
RUN wget -qO /etc/apt/sources.list.d/lunarg-vulkan-1.2.148-bionic.list https://packages.lunarg.com/vulkan/1.2.148/lunarg-vulkan-1.2.148-bionic.list

RUN apt-get -yq update && apt-get -yq install cmake ninja-build libopenal-dev libreadline6-dev libpng-dev libjpeg62-dev \
	liblua5.1-0-dev libjansson-dev libsdl2-dev libfreetype6-dev valgrind g++-5 g++-10 vulkan-sdk

COPY llvm.sh /scripts/llvm.sh

RUN /scripts/llvm.sh 9 && apt-get -yq install clang-tidy-9 libc++-9-dev libc++abi-9-dev  && rm -rf /scripts

COPY appimagetool /tools/appimagetool

RUN ln -s /tools/appimagetool/AppRun /usr/bin/appimagetool

ENTRYPOINT [ "/bin/bash" ]
