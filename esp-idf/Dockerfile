ARG IDF_VERSION=4.4
ARG NIM_VERSION=1.6.20

FROM espressif/idf:release-v${IDF_VERSION} as base

ARG DEBIAN_FRONTEND=noninteractive

RUN : \
  && apt-get update \
  && apt-get install -y \
        zsh \
        bison \
        psmisc \
        build-essential \
        cmake \
        vim \
        gcc \
        iputils-ping \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/*

# Ccache is installed, enable it by default
ENV IDF_CCACHE_ENABLE=1

RUN : \
  && git clone https://github.com/asdf-vm/asdf.git $HOME/.asdf --branch v0.8.1

RUN : \
    && echo '. /opt/esp/idf/export.sh' >> /etc/bash.bashrc \
    && echo '. /opt/esp/idf/export.sh' >> /etc/zsh/zshrc \
    && echo '. $HOME/.asdf/asdf.sh' >> /etc/bash.bashrc \
    && echo '. $HOME/.asdf/asdf.sh' >> /etc/zsh/zshrc

ARG TARGETPLATFORM
ARG TARGETOS
ARG TARGETARCH
ARG BUILDPLATFORM
ARG BUILDOS
ARG BUILDARCH

ENV CACHE_NONCE=1

RUN : \
  && cd $HOME/ \
    && echo "IDF_VERSION: " ${IDF_VERSION} \
    && echo "NIM: " ${NIM_VERSION} \
    && asdf plugin-add nim \
    && asdf install nim ${NIM_VERSION} \
    && echo "done"
