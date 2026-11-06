FROM steamcmd/steamcmd:debian-trixie AS download

ENV HOME=/root
ENV PZ_INSTALL_DIR=/opt/pzserver
RUN mkdir -p ${PZ_INSTALL_DIR} && \
    for i in 1 2 3 4 5; do \
      if steamcmd +force_install_dir ${PZ_INSTALL_DIR} +login anonymous +app_update 380870 validate +quit && \
         [ -f "${PZ_INSTALL_DIR}/start-server.sh" ]; then \
        exit 0; \
      fi; \
      if [ "$i" -lt 5 ]; then \
        echo "Attempt $i failed, retrying in 5s..." >&2; \
        sleep 5; \
      fi; \
    done; \
    echo "Failed to install Project Zomboid server after 5 attempts." >&2; \
    exit 1


# Runtime image

FROM debian:trixie-slim

ENV PZ_INSTALL_DIR=/opt/pzserver
COPY --from=download /opt/pzserver /opt/pzserver

RUN useradd -m pzuser && \
    chown -R pzuser:pzuser ${PZ_INSTALL_DIR} && \
    apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

USER pzuser

WORKDIR ${PZ_INSTALL_DIR}

ENTRYPOINT [ "./start-server.sh" ]