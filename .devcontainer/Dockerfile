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

# RUN curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o /root/.git-completion.bash && \
#     echo '' && \
#     echo 'if [ -f ~/.git-completion.bash ]; then' >> /root/.bashrc && \
#     echo '  . ~/.git-completion.bash' >> /root/.bashrc && \
#     echo 'fi' >> /root/.bashrc && \
#     echo ''


# Installing Python
RUN apt update && \
    apt-get install python3-venv -y

    

# Installing PlatformIO
RUN   mkdir -p /usr/local/bin && \ 
      ln -s ~/.platformio/penv/bin/platformio /usr/local/bin/platformio && \
      ln -s ~/.platformio/penv/bin/pio /usr/local/bin/pio && \
      ln -s ~/.platformio/penv/bin/piodebuggdb /usr/local/bin/piodebuggdb

# install VS Code (code-server)
# RUN curl -fsSL https://code-server.dev/install.sh | sh

# # install VS Code extensions
# RUN code --install-extension platformio.platformio-ide && \
#     code --install-extension thecreativedodo.usbip-connect && \
#     code --install-extension ms-vscode.cpptools


EXPOSE 5000

# COPY installExtensions.sh /
# RUN chmod +x /installExtensions.sh
# ENTRYPOINT [ "/installExtensions.sh" ]
# CMD [ "sleep", "infinity" ]


# ENTRYPOINT [ "/.vscode/installExtensions.sh" ]

# # ENTRYPOINT [ "/docker-entrypoint.sh" ]
CMD [ "sleep", "infinity" ]