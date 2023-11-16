FROM ubuntu:22.04

SHELL ["/bin/bash", "-c"]

ENV CLANG_VERSION 1:14.*
ENV DOXYGEN_VERSION 1.9.1*
ENV ASCIIDOCTOR_VERSION 2.0.16*
ENV ASCIIDOCTOR_DIAGRAM_VERSION 2.2.8
ENV ASCIIDOCTOR_PDF_VERSION 2.3.7
ENV CODERAY_VERSION 1.1.3
ENV ROUGE_VERSION 4.1.0

# Installing basic tools
RUN apt update && apt upgrade -y && \
    apt install -y \
    build-essential \
    udev \
    git \
    zip \
    unzip \
    bzip2 \
    nano \
    curl \
    graphviz \
    default-jre \
    wget \
    clang=${CLANG_VERSION} \
    clang-tools=${CLANG_VERSION} \
    clang-tidy=${CLANG_VERSION} \
    clang-format=${CLANG_VERSION} \
    lldb \
    lld \
    perl \
    doxygen=${DOXYGEN_VERSION} \
    minicom \
    python3-pip \
    libusb-dev \
    usbutils && \
    apt install -y linux-tools-virtual hwdata && \
    update-alternatives --install /usr/local/bin/usbip usbip `ls /usr/lib/linux-tools/*/usbip | tail -n1` 20 && \
    apt install -y libncurses5 && \
    apt install -y linux-tools-generic && \
    apt install -y ruby && \
    apt install -y asciidoctor=${ASCIIDOCTOR_VERSION} && \
    gem install asciidoctor-pdf -v ${ASCIIDOCTOR_PDF_VERSION} && \
    gem install rouge -v ${ROUGE_VERSION} && \
    gem install coderay -v ${CODERAY_VERSION} && \
    gem install pygments.rb && \
    gem install asciidoctor-diagram -v ${ASCIIDOCTOR_DIAGRAM_VERSION} && \
    apt clean


# Installing Python, and making shure it can be used by PlatformIO
RUN apt update && \
    apt-get install python3-venv -y

# # Installing PlatformIO
# RUN   mkdir -p /usr/local/bin && \ 
#       ln -s ~/.platformio/penv/bin/platformio /usr/local/bin/platformio && \
#       ln -s ~/.platformio/penv/bin/pio /usr/local/bin/pio && \
#       ln -s ~/.platformio/penv/bin/piodebuggdb /usr/local/bin/piodebuggdb

# EXPOSE 5000

CMD [ "sleep", "infinity" ]