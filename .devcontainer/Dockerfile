FROM ubuntu:20.04

ENV HOME /root

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
    && apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# timezone
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        tzdata \
    && apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/* \
    && ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    && echo 'Asia/Tokyo' >/etc/timezone

# tool
RUN apt-get update \
    && apt-get install -y \
        make build-essential \
        git openssh-client sudo vim bash-completion rsync wget curl python3 \
    && apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# haskell toolchain
RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org

RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid 1000 -m vscode \
    && echo vscode ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/vscode \
    && chmod 0440 /etc/sudoers.d/vscode

USER vscode
ENV HOME /home/vscode

RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | BOOTSTRAP_HASKELL_NONINTERACTIVE=1 sh

ENV PATH "$PATH:/home/vscode/.ghcup/bin"

RUN ghcup install stack recommended \
    && ghcup install hls recommended

USER root

# build dependencies
RUN apt-get update \
    && apt-get install -y \
        libgmp3-dev libz-dev libncurses-dev pkg-config \
    && apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

USER vscode