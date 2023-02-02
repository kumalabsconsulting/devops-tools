#FROM kumalabsconsulting/base-workspace:0.1
FROM redbeard28/base-devspace:0.2.1 AS BUILDER


# Set environmental variables for Code-server
ARG CODESERVER_VERSION="4.8.3"


# Set env variables for Cronicle and Static file server
ENV CRONICLE_VERSION="v0.9.16" \
    CRONICLE_DIR="/home/abc/apps/cronicle" \
    STATIC_FILESERVER_DIR="/home/abc/apps/static-file-server"

# Clone Cronicle & create node environment
RUN echo "------------------------------------------------------ Cronicle" \
	&& git clone --branch $CRONICLE_VERSION https://github.com/jhuckaby/Cronicle.git $CRONICLE_DIR \
    && rm -rf $CRONICLE_DIR/.git \
    && rm -rf $CRONICLE_DIR/.github \
    && cd $CRONICLE_DIR && nodeenv --node=12.18.3 --npm=6.0.0 env \
    && cd $CRONICLE_DIR && . env/bin/activate && npm install; node bin/build.js dist

# Copy Cronicle config
COPY --chown=abc:abc cronicle-config.json $CRONICLE_DIR/conf/config.json

# Create node environment & install Static file server
RUN echo "------------------------------------------------------ Static file server" \
    && mkdir /home/static-server \
	&& mkdir -p $STATIC_FILESERVER_DIR \
	&& cd $STATIC_FILESERVER_DIR && nodeenv --node=16.17.0 --npm=6.0.0 env \
	&& cd $STATIC_FILESERVER_DIR && . env/bin/activate &&  npm install -g serve@14.0.1

USER root
RUN echo "fs.inotify.max_user_watches=524288" >> /etc/sysctl.conf
RUN rm -rf /home/docs/*
COPY docs/. /home/docs/

USER abc
COPY supervisord/* /etc/supervisord/

## Install Code-server 
RUN echo "------------------------------------------------------ code-server" \
	&& cd \tmp \
    && curl -fOL https://github.com/coder/code-server/releases/download/v$CODESERVER_VERSION/code-server_${CODESERVER_VERSION}_amd64.deb \
    && sudo dpkg -i code-server_${CODESERVER_VERSION}_amd64.deb \
    && rm /tmp/code-server_${CODESERVER_VERSION}_amd64.deb

# Install codeserver extensions 
RUN echo "------------------------------------------------------ common extensions" \
    && code-server --install-extension  mhutchie.git-graph \
    && code-server --install-extension  redhat.vscode-yaml \
    && code-server --install-extension  mechatroner.rainbow-csv \
    && code-server --install-extension  formulahendry.auto-close-tag \
    && code-server --install-extension  formulahendry.auto-rename-tag \
    && code-server --install-extension  anteprimorac.html-end-tag-labels \
    && code-server --install-extension  abusaidm.html-snippets \
    && code-server --install-extension  sndst00m.vscode-native-svg-preview \
    && code-server --install-extension  pranaygp.vscode-css-peek \
    && code-server --install-extension  anseki.vscode-color \
    && echo "------------------------------------------------------ themes" \
    && code-server --install-extension  armandphilippot.coldark \
    && code-server --install-extension  RobbOwen.synthwave-vscode \
    && code-server --install-extension  nadim-vscode.infinity-dark-theme \
    && code-server --install-extension  emroussel.atomize-atom-one-dark-theme \
    && code-server --install-extension  teabyii.ayu \
    && code-server --install-extension  wesbos.theme-cobalt2 \
    && code-server --install-extension  github.github-vscode-theme \
    && code-server --install-extension  armandphilippot.coldark \
    && code-server --install-extension  radiolevity.search-lights \
    && code-server --install-extension  vladeeg.vscode-theme-vlight \
    && code-server --install-extension  akamud.vscode-theme-onelight \
    && code-server --install-extension  akamud.vscode-theme-onedark \
    && code-server --install-extension  rubjo.ultimate-dark-neo \
    && code-server --install-extension  sainnhe.edge \
    && code-server --install-extension  circleci.circleci \
    && code-server --install-extension  lakshitsomani.best-themes-redefined \
    && code-server --install-extension  usernamehw.prism \
    && code-server --install-extension  ms-azuretools.vscode-docker \
    && echo "------------------------------------------------------ icons" \
    && code-server --install-extension  emroussel.atom-icons \
    && code-server --install-extension  laurenttreguier.vscode-simple-icons 

#Set default codeserver terminal
ENV SHELL=/bin/zsh
ENV TERM=xterm
ENV EDITOR=mcedit
# Set default codeserver theme
COPY --chown=abc:abc settings.json /home/abc/.local/share/code-server/User/settings.json
