ARG BUILD_FROM
FROM john30/ebusd:latest AS ebusd-source

FROM $BUILD_FROM

ENV LANG C.UTF-8

# Install runtime dependencies for ebusd
RUN apt-get update && apt-get install -y --no-install-recommends \
    libmosquitto1 \
    netcat-openbsd \
    && rm -rf /var/lib/apt/lists/*

# Copy ebusd binaries and default config from official image
COPY --from=ebusd-source /usr/bin/ebusd /usr/bin/ebusd
COPY --from=ebusd-source /usr/bin/ebusctl /usr/bin/ebusctl
COPY --from=ebusd-source /etc/ebusd /etc/ebusd

COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]

HEALTHCHECK --interval=5m --timeout=3s --start-period=90s \
   CMD nc -z 127.0.0.1 8888 || exit 1
