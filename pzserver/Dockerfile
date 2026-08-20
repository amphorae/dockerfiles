FROM steamcmd/steamcmd:debian-trixie AS download

ENV HOME=/root
ENV PZ_INSTALL_DIR=/opt/pzserver
RUN mkdir -p ${PZ_INSTALL_DIR} && \
    steamcmd +quit; \
    for i in 1 2 3 4 5; do \
      if steamcmd +force_install_dir ${PZ_INSTALL_DIR} +login anonymous +app_update 380870 validate +quit; then \
        break; \
      else \
        echo "Attempt $i failed, retrying in 5s..."; sleep 5; \
      fi; \
    done


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