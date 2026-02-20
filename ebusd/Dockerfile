ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

# Install build dependencies
RUN apk add --no-cache \
    build-base \
    cmake \
    git \
    openssl-dev \
    curl \
    jq

# Fetch latest ebusd release tag dynamically
RUN LATEST_TAG=$(curl -s https://api.github.com/repos/john30/ebusd/releases/latest | jq -r .tag_name) && \
    echo "Latest ebusd version: $LATEST_TAG" && \
    git clone https://github.com/john30/ebusd.git /tmp/ebusd && \
    cd /tmp/ebusd && \
    git checkout "$LATEST_TAG" && \
    cmake . && \
    make && \
    make install

# Cleanup
RUN rm -rf /tmp/ebusd && \
    apk del git cmake build-base jq curl

COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]

HEALTHCHECK --interval=5m --timeout=3s --start-period=90s \
   CMD nc -z 127.0.0.1 8888 || exit 1
