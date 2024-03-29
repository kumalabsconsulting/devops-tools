FROM redbeard28/debian-workspace:0.2.1 AS BUILDER

ENV FRP_VERSION=0.45.0

COPY supervisord/* /etc/supervisord/
RUN rm -rf /home/docs/*
COPY docs/. /home/docs/
# Make image add dir for logs
USER root

RUN echo "------------------------------------------------------ log dir for workspaces" \
    && mkdir /var/log/workspace /home/wrk_ui \
    && chown abc /var/log/workspace /home/wrk_ui \
    && mkdir /home/examples/ \
    && chown -R abc:abc /home/examples

USER abc
ENV DEBIAN_FRONTEND=noninteractive
COPY --chown=abc:abc mkdocs-material /home/examples/mkdocs-material
# Install workspace tools and applications
RUN sudo apt-get -y update \
    && sudo apt-get install -y socat s3fs net-tools dnsutils \
    && echo "------------------------------------------------------ install mkdocs" \
    && sudo apt-get install -y mkdocs \
    && pip install mkdocs-material \
    && pip install mkdocs-macros-plugin \
    && pip install mkdocs-mermaid2-plugin mkdocs-include-markdown-plugin mkdocs-awesome-pages-plugin \
    && echo "------------------------------------------------------ install filebrowser" \
	&& cd /tmp && wget https://github.com/filebrowser/filebrowser/releases/download/v2.21.1/linux-amd64-filebrowser.tar.gz \
    && mkdir /tmp/filebrowser \
    && tar xvf /tmp/linux-amd64-filebrowser.tar.gz -C /tmp/filebrowser \
    && chmod +x /tmp/filebrowser/filebrowser \
    && mkdir /home/abc/apps/filebrowser/ \
    && mv /tmp/filebrowser/filebrowser /home/abc/apps/filebrowser/ \
    && rm -rf /tmp/filebrowser \
    && rm /tmp/linux-amd64-filebrowser.tar.gz \
    && echo "------------------------------------------------------ install frp" \
    && cd /tmp && wget https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz \
    && tar xvf /tmp/frp_${FRP_VERSION}_linux_amd64.tar.gz \
    && mv /tmp/frp_${FRP_VERSION}_linux_amd64 /home/abc/apps/frp \
    && rm /tmp/frp_${FRP_VERSION}_linux_amd64.tar.gz \
    && sudo apt-get install -y timelimit

# Copy Filebrowser config (it changes filebrowser standard port and host)
COPY --chown=abc:abc filebrowser.json /home/abc/apps/filebrowser/.filebrowser.json

