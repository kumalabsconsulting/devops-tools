# Base-Workspace

L'idée est d'avoir une image de base sous debian stable avec un seul utilisateur **abc**.

Il n'a que les droits sudo sur le gestionnaire apt.

## Push to Docker Hub
```bash
docker login
docker push redbeard28/base-workspace:TAG_NAME
```


## Pull from Docker Hub
```bash
docker pull redbeard28/base-workspace:TAG_NAME
```


## Features

- [**Terminal**](https://github.com/tsl0922/ttyd) - secure browser-based terminal.
- [**FileBrowser**](https://github.com/filebrowser/filebrowser)  - manage files and folders inside the workspace, and exchange data between local environment and the workspace
- [**Ungit**](https://github.com/FredrikNoren/ungit) - rings user friendliness to git without sacrificing the versatility of it.
- **Ubuntu 20.4** with the following CLI apps
    - [Zsh](https://www.zsh.org/), [Oh my Zsh](https://ohmyz.sh/)
    - Python 3, Pip 
    - Node/nodeenv
    - curl, wget, telnet, jq
    - **Git:** git, git-flow, lazygit 
    - **File browsers:** mc
    - **Text editors:** nano, vim, mcedit
    - **System monitors:** ncdu, htop, glances, vizex
    - **Process Control:** supervisord
    - **Job scheduler:** cron
    - **Terminal multiplexer:** tmux 

## Inspiration

[__alnoda-wrk__](https://pypi.org/project/alnoda-wrk/) installed, and workspace initialised. 