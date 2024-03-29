FROM debian:stable

COPY supervisord/unified-supervisord.conf supervisord/supervisord.conf /etc/supervisord/
COPY mc.ini /home/abc/.config/mc/ini
COPY zsh-in-docker.sh /tmp/zsh-in-docker.sh

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \ 
    && echo "------------------------------------------------------ Common" \
    && apt-get install -y sudo curl wget telnet jq dnsutils apt-utils \
    && apt-get install -y software-properties-common \
    && apt-get install -y zip gzip tar \
    && echo "------------------------------------------------------ User" \
    && useradd -u 8877 abc \
    && chown -R abc /home \
    && mkdir -p /home/abc \
    && chown -R abc /home/abc \
    && mkdir -p /home/abc/apps \
    && chown -R abc /home/abc/apps \
    && echo "------------------------------------------------------ Nix folder and conf" \
    && mkdir -m 0750 /nix && chown abc /nix \
    && echo "------------------------------------------------------ docker systemctl replacement" \
    && wget https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/master/files/docker/systemctl3.py -O /usr/local/bin/systemctl  \
    && chown -R abc /usr/local/bin/systemctl \ 
    && echo "------------------------------------------------------ Python" \
    && apt-get install -y python3-distutils \
    && apt-get install -y python3-pip \
    && pip install --upgrade pip \
    && pip install --upgrade setuptools \
    && pip install --upgrade distlib \
    &&  apt-get install -y python-is-python3 \
    && mkdir -p /usr/bin/pip3.8 \
    && chown abc /usr/bin/pip3.8 \
    && echo "------------------------------------------------------ Allow users to install packages with apt" \
    && echo "# Allow non-admin users to install packages" >> /etc/sudoers \
    && echo "abc ALL = NOPASSWD : /usr/bin/apt, /usr/bin/apt-get, /usr/bin/aptitude, /usr/bin/add-apt-repository, /usr/local/bin/pip, /usr/local/bin/systemctl, /usr/bin/dpkg, /usr/sbin/dpkg-reconfigure" >> /etc/sudoers \
    && chown abc /etc/apt/sources.list.d \
    && chown abc /etc/apt/trusted.gpg.d \
    && echo "------------------------------------------------------ GIT" \
    && apt-get install -y git git-flow \
    && wget -P /tmp https://github.com/jesseduffield/lazygit/releases/download/v0.34/lazygit_0.34_Linux_x86_64.tar.gz \
    && mkdir /tmp/lazygit  && tar -xzf /tmp/lazygit_0.34_Linux_x86_64.tar.gz --directory /tmp/lazygit \
    && chmod +x /tmp/lazygit/lazygit \
    && mv /tmp/lazygit/lazygit /usr/bin/ \
    && rm /tmp/lazygit_0.34_Linux_x86_64.tar.gz \
    && rm -rf /tmp/lazygit \
    && echo "------------------------------------------------------ Nodeenv" \
    && pip install nodeenv \
    && apt-get install -y yarn \
    && echo "------------------------------------------------------ Node env with pm2" \
    && mkdir -p /home/abc/apps/node \
    && chown -R abc /home/abc/apps/ \
    && cd /home/abc/apps/node && nodeenv --node=18.9.0 env \
    && cd /home/abc/apps/node && . env/bin/activate && npm install -g pm2@5.2.0 \
    && echo "------------------------------------------------------ Cron" \
    && apt-get install -y cron \
    && mkdir -p /var/log/supervisord/ \
    && chown -R abc /var/spool/cron/crontabs \
    && chown -R abc /var/log \
    && chmod gu+rw /var/run \
    && chmod gu+s /usr/sbin/cron \
    && echo "# Allow cron for user abc" >> /etc/sudoers \
    && echo "abc ALL = NOPASSWD : /usr/sbin/cron " >> /etc/sudoers \
    && echo "------------------------------------------------------ Supervisor" \
    && apt-get remove -y cmdtest \
    && apt-get install -y supervisor \
    && pip3 install supervisor==4.2.2 \
    && apt-get -y update \
    && apt-get install -y systemd \
    && chown -R abc /etc/supervisord/ \
    && chown -R abc /var/log/supervisord/ \
    && echo "------------------------------------------------------ ZSH root" \
    && HOME=/root \
    && chmod +x /tmp/zsh-in-docker.sh \
    && /tmp/zsh-in-docker.sh \
    -t https://github.com/pascaldevink/spaceship-zsh-theme \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -a 'export LS_COLORS="$LS_COLORS:ow=1;34:tw=1;34:"' \
    -a 'SPACESHIP_USER_SHOW="false"' \
    -a 'SPACESHIP_TIME_SHOW="true"' \
    -a 'SPACESHIP_TIME_COLOR="grey"' \
    -a 'SPACESHIP_DIR_COLOR="cyan"' \
    -a 'SPACESHIP_GIT_SYMBOL="⇡"' \
    -a 'SPACESHIP_BATTERY_SHOW="false"' \
    -a 'if [[ $(pwd) != /root  ]]; then cd /root; fi  # Set starting dir to /root ' \
    -a 'hash -d r=/root' \
    -p git \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions \
    -p https://github.com/zsh-users/zsh-history-substring-search \
    -p https://github.com/zsh-users/zsh-syntax-highlighting \
    -p 'history-substring-search' \
    -p https://github.com/bobthecow/git-flow-completion \
    -a 'bindkey "\$terminfo[kcuu1]" history-substring-search-up' \
    -a 'bindkey "\$terminfo[kcud1]" history-substring-search-down' \
    && printf '%s\n%s\n' "export ZSH_DISABLE_COMPFIX=true" "$(cat /root/.zshrc)" > /root/.zshrc \
    && echo "------------------------------------------------------ ZSH abc" \
    && mkdir -p /home/project \
    && HOME=/home/abc \
    && /tmp/zsh-in-docker.sh \
    -t https://github.com/pascaldevink/spaceship-zsh-theme \
    -a 'DISABLE_UPDATE_PROMPT="true"' \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -a 'export LS_COLORS="$LS_COLORS:ow=1;34:tw=1;34:"' \
    -a 'SPACESHIP_USER_SHOW="true"' \
    -a 'SPACESHIP_TIME_SHOW="true"' \
    -a 'SPACESHIP_TIME_COLOR="grey"' \
    -a 'SPACESHIP_DIR_COLOR="cyan"' \
    -a 'SPACESHIP_GIT_SYMBOL="⇡"' \
    -a 'SPACESHIP_BATTERY_SHOW="false"' \
    -a 'if [[ $(pwd) != /home/project  ]]; then cd /home/project; fi  # Set starting dir to /home/project ' \
    -a 'hash -d p=/home/project' \
    -p git \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions \
    -p https://github.com/zsh-users/zsh-history-substring-search \
    -p https://github.com/zsh-users/zsh-syntax-highlighting \
    -p 'history-substring-search' \
    -p https://github.com/bobthecow/git-flow-completion \
    -a 'bindkey "\$terminfo[kcuu1]" history-substring-search-up' \
    -a 'bindkey "\$terminfo[kcud1]" history-substring-search-down' \
    && rm /tmp/zsh-in-docker.sh \
    && printf '%s\n%s\n' "export ZSH_DISABLE_COMPFIX=true" "$(cat /home/abc/.zshrc)" > /home/abc/.zshrc \
    && echo "------------------------------------------------------ Code editors" \
    && apt-get install -y nano vim \
    && apt-get install -y tilde \
    && echo "------------------------------------------------------ File browsers: MC" \
    && apt-get install -y mc \
    && echo "------------------------------------------------------ multitasking in a terminal " \
    && apt-get install -y tmux \
    && echo "------------------------------------------------------ Sys monitoring: Glances, Vizex" \
    && apt-get install -y ncdu htop \
    && pip install glances==3.2.5 \
    && echo "------------------------------------------------------ Web-based terminal" \
    && cd /tmp && wget https://github.com/tsl0922/ttyd/releases/download/1.7.1/ttyd.x86_64 \
    && mv ttyd.x86_64 /usr/bin/ttyd \
    && chmod +x /usr/bin/ttyd \
    && echo "------------------------------------------------------ User" \
    && chown abc /etc/timezone \
    && chown abc /home/project \
    && mkdir -p /home/abc/bin \ 
    && chown abc /home/abc/bin \
    && mkdir -p /home/abc/.local/bin \ 
    && chown abc /home/abc/.local && chown abc /home/abc/.local/bin \
    && chown abc /home/abc/.local && chown abc /home/abc/.local/bin  \
    && find /home -type d | xargs -I{} chown -R abc {} \
    && find /home -type f | xargs -I{} chown abc {} \
    && echo "------------------------------------------------------ Aliases" \
    && echo 'alias python="python3"' >> /root/.zshrc \
    && echo 'alias python="python3"' >> /home/abc/.zshrc \
    && echo 'alias pm2="cd /home/abc/apps/node && . env/bin/activate && pm2"' >> /home/abc/.zshrc \
    && echo 'alias lg="lazygit"' >> /home/abc/.zshrc \
    && echo "------------------------------------------------------ Clean" \
    && apt-get -y autoremove \
    && apt-get -y clean \
    && apt-get -y autoclean \
    && rm -rf /home/abc/.oh-my-zsh/.git \
    && rm -rf /home/abc/.oh-my-zsh/.github \
    && rm -rf /home/abc/.oh-my-zsh/custom/plugins/git-flow-completion/.git \
    && rm -rf /home/abc/.oh-my-zsh/custom/plugins/zsh-autosuggestions/.git \
    && rm -rf /home/abc/.oh-my-zsh/custom/plugins/zsh-completions/.git \
    && rm -rf /home/abc/.oh-my-zsh/custom/plugins/zsh-history-substring-search/.git \
    && rm -rf /home/abc/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/.git \
    && rm -rf /home/abc/.oh-my-zsh/custom/themes/spaceship-zsh-theme/.git 

USER abc

RUN git config --global credential.helper cache \
    && echo "------------------------------------------------------ Nix" \
    && curl -L https://nixos.org/nix/install > /tmp/nix.sh \
    && chmod +x /tmp/nix.sh \
    && sh /tmp/nix.sh --no-daemon  \
    && rm /tmp/nix.sh \
    && echo "------------------------------------------------------ Pipx" \
    && sudo apt-get -y install python3-venv \
    && python3 -m pip install --user pipx \
    && python3 -m pipx ensurepath 

ENV PATH="/home/abc/bin:${PATH}"
ENV PATH="/home/abc/.local/bin:${PATH}"
ENV PATH="/home/abc/.nix-profile/bin:${PATH}"

RUN echo "------------------------------------------------------ after PATH updates" \
    && pipx install rich-cli \
    && echo 'alias p="rich"' >> /home/abc/.zshrc \
    && nix-env -iA cachix -f https://cachix.org/api/v1/install    

###### ENTRY

# note! this will have consequences only when started as root (docker run ... --user root ...)  
# systemctl start systemd-journald 
#   I remove this from entrypoint, as it is not used significantly, but slows down the start

# this entrypoint should be the same for all images that are built on top of this one
ENTRYPOINT /etc/init.d/cron start; supervisord -c "/etc/supervisord/unified-supervisord.conf"
