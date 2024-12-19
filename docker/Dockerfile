ARG alpine_version \
    node_version

FROM gitlab-registry-internal.spirit-dev.net/infrastructure/docker/alpine:${alpine_version}

ARG alpine_version \
    node_version \
    TARGETPLATFORM

ENV UMASK=0002

LABEL alpine_version="${alpine_version}" \
    node_version="${node_version}" \
    spiritdev_official="yes" \
    layer="1" \
    from="gitlab-registry-internal.spirit-dev.net/infrastructure/docker/alpine" \
    maintainer="bordat.jean@gmail.com"

USER root

SHELL ["/bin/sh", "-o", "pipefail", "-c"]

WORKDIR /app

# hadolint ignore=DL3008,DL3015,SC2086,SC2155
RUN \
apk update && \
  apk --no-cache update && \
  apk --no-cache add \
    redis \
    bash && \
  case "${TARGETPLATFORM}" in \
    'linux/amd64') export ARCH='linux-x64' ;; \
  esac && \
  apk del --no-cache jq && \
  rm -rf /var/cache/apk/* && \
  chmod -R u=rwX,go=rX /app && \
  printf "umask %d" "${UMASK}" >> /etc/profile && \
  update-ca-certificates

USER apps

COPY docker/assets/entrypoint.sh /entrypoint.sh
CMD ["/entrypoint.sh"]
