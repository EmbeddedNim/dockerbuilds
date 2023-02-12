FROM espressif/idf:release-v4.4 as base

ARG DEBIAN_FRONTEND=noninteractive

RUN : \
  && apt-get update \
  && apt-get install -y \
        zsh \
        bison \
        psmisc \
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
    && wget https://github.com/nim-lang/nightlies/releases/download/latest-version-1-6/${NIM_PLATFORM}.tar.xz \
    && tar xf ${NIM_PLATFORM}.tar.xz \
    && mv nim-* /usr/local/ \
    && for b in atlas nim nimble nim_dbg nim-gdb nimgrep nimpretty nimsuggest testament; \
      do ln -sf /usr/local/nim-*/bin/$b /usr/local/bin/; \
    done
