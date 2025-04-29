ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.8.2
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        sudo=1.9.13p3-1+deb12u1 \
        locales=2.36-9+deb12u10 \
        cups=2.4.2-3+deb12u8 \
        avahi-daemon=0.8-10+deb12u1 \
        libnss-mdns=0.15.1-3 \
        dbus=1.14.10-1~deb12u1 \
        colord=1.4.6-2.2 \
        printer-driver-all=0.20210903 \
        printer-driver-gutenprint=5.3.4.20220624T01008808d602-1 \
        openprinting-ppds=20230202-1 \
        hpijs-ppds=3.22.10+dfsg0-2 \
        hp-ppd=0.9+nmu1  \
        hplip=3.22.10+dfsg0-2 \
        printer-driver-foo2zjs=20200505dfsg0-2 \
        printer-driver-cups-pdf=3.0.1-14 \
        gnupg2=2.2.40-1.1 \
        lsb-release=12.0-1 \
        nano=7.2-1+deb12u1 \
        samba=2:4.17.12+dfsg-0+deb12u1 \
        bash-completion=1:2.11-6 \
        procps=2:4.0.2-3 \
        whois=5.5.17 \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

COPY rootfs /

# Add user and disable sudo password checking
RUN useradd \
  --groups=sudo,lp,lpadmin \
  --create-home \
  --home-dir=/home/print \
  --shell=/bin/bash \
  --password="$(mkpasswd print)" \
  print \
&& sed -i '/%sudo[[:space:]]/ s/ALL[[:space:]]*$/NOPASSWD:ALL/' /etc/sudoers

EXPOSE 631

RUN chmod a+x /run.sh

CMD ["/run.sh"]

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Jochen Schalanda <jochen+home-assistant@schalanda.name>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.authors="Jochen Schalanda <jochen+home-assistant@schalanda.name>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

