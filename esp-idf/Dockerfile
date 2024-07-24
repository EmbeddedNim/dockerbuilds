ARG IDF_VERSION=4.4
ARG NIM_VERSION=1.6.20

FROM espressif/idf:release-v${IDF_VERSION} AS base

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
        iputils-ping \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/*

# Ccache is installed, enable it by default
ENV IDF_CCACHE_ENABLE=1

RUN : \
    && echo '. /opt/esp/idf/export.sh' >> /etc/bash.bashrc \
    && echo '. /opt/esp/idf/export.sh' >> /etc/zsh/zshrc

ARG TARGETPLATFORM
ARG TARGETOS
ARG TARGETARCH
ARG BUILDPLATFORM
ARG BUILDOS
ARG BUILDARCH
ARG NIM_VERSION
ARG IDF_VERSION

ENV CACHE_NONCE=3

RUN : \
  && cd $HOME/ \
    && case ${TARGETARCH} in \
         "arm64")        TARCH=arm64  ;; \
         "amd64")        TARCH=x64  ;; \
         "arm")          TARCH=arm  ;; \
         "386")          TARCH=x32  ;; \
         *)              TARCH=${TARGETARCH} ;; \
       esac \
    && NIM_PLATFORM=${TARGETOS}_${TARCH} \
    && env \
    && echo "NIM_PLATFORM: " ${NIM_PLATFORM} \
    && echo "TARGETARCH: " ${TARGETARCH} \
    && echo "downloading: " https://nim-lang.org/download/nim-${NIM_VERSION}-${NIM_PLATFORM}.tar.xz \
    && wget https://nim-lang.org/download/nim-${NIM_VERSION}-${NIM_PLATFORM}.tar.xz \
    && tar xf nim-${NIM_VERSION}-${NIM_PLATFORM}.tar.xz \
    && mv -v nim-* /usr/local/ \
    && for b in atlas nim nimble nim_dbg nim-gdb nimgrep nimpretty nimsuggest testament; \
      do ln -sf /usr/local/nim-*/bin/$b /usr/local/bin/; \
    done
