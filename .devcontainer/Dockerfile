FROM node:25.8.0-trixie-slim

RUN apt-get update && apt-get install -y --no-install-recommends sudo:

ARG CONTAINER_WORKDIR
ENV CONTAINER_WORKDIR=$CONTAINER_WORKDIR
WORKDIR $CONTAINER_WORKDIR

ARG CONTAINER_USER
ENV CONTAINER_USER=$CONTAINER_USER
RUN useradd -ms /bin/bash $CONTAINER_USER
RUN echo $CONTAINER_USER:password | chpasswd
RUN echo "$CONTAINER_USER   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# ユーザID等をホストに合わせる
COPY --chown=$CONTAINER_USER --chmod=755 .devcontainer/entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/bin/bash"]

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    git \
    curl \
    locales-all \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# zenn-cli をグローバルインストール
RUN npm install -g zenn-cli@latest
