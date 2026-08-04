FROM steamcmd/steamcmd:debian-trixie AS download

ENV HOME=/root
ENV PZ_INSTALL_DIR=/opt/pzserver
RUN mkdir -p ${PZ_INSTALL_DIR}
COPY update_zomboid.txt ${HOME}/update_zomboid.txt
RUN steamcmd +quit && \
    steamcmd +runscript ${HOME}/update_zomboid.txt


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