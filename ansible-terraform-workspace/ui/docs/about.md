
## About

| Name      | Ansible-Terraform workspace                          |
| ----------- | ------------------------------------ |
| version       | 4.0 |
| author       | bluxmit |
| created    | 2022-09-17 |
| tags       | `terraform` ` ansible` ` infra-visualisation` ` playbook-monitoring` |

## Description  
# Base devspace
Containerized development, execution and admin environment for Ansible and Terraform. 
Create, provision, visualize and manage infrastructures, schedule maintenance tasks.


## Lineage 
This workspace has features of the following workspaces (build history)

| Layer      | Name                          | Version      | Tags      |
| ----------- | --------------------------- | ----------- | ----------- |
| 5       | [Ansible-Terraform workspace](https://docs.alnoda.org/workspaces/ansible-terraform-workspace/)        | 4.0      |  `terraform` ` ansible` ` infra-visualisation` ` playbook-monitoring`     |
| 4       | [Extended workspace](https://docs.alnoda.org/workspaces/extended-workspace/)        | 4.0      |  `task-scheduler` ` static-file-server`     |
| 3       | [Code-server workspace](https://docs.alnoda.org/workspaces/codeserver-workspace/)        | 4.0      |  `web-ide` ` codeserver`     |
| 2       | [Base devspace](https://github.com/bluxmit/alnoda-workspaces/blob/main/workspaces/base-devspace/README.md)        | 4.0      |  `workspace-ui` ` filebrowser` ` ungit`     |
| 1       | [Ubuntu workspace](https://docs.alnoda.org/ubuntu-workspace/)        | 4.0      |  `multi-process` ` supervisor` ` zsh` ` nix` ` cron` ` pm2` ` tmux` ` git` ` lazygit` ` python` ` pip` ` pipx` ` nodeenv` ` mc` ` nano` ` vim`     |


## UI tabs and ports
UI shortcuts for apps and their respective ports (allowed port range is 8021-8040)

| Port      | Application                          | Tab      |
| ----------- | ------------------------------------ | ----------- |
| 8021       | IDE        | home      |
| 8022       | Terminal        | home      |
| 8022       | Terminal        | admin      |
| 8023       | Filebrowser        | home      |
| 8024       | Ungit        | home      |
| 8025       | Workspace settings        | admin      |
| 8026       | M.Commander        | admin      |
| 8027       | Process monitor        | admin      |
| 8028       | Static File Server        | home      |
| 8029       | Cronicle        | home      |
| 8029       | Cronicle        | admin      |
| 8030       | My app on port 8030        | my_apps      |
| 8031       | My app on port 8031        | my_apps      |
| 8032       | Ansible Ara        | home      |
| 8033       | Blast Radius        | home      |
| 8034       | Terraform Rover        | home      |


## Apps & services
Commads used to start apps and services in the workspace

| Application      | Command                          |
| ----------- | ------------------------------------ |
| ansibleara       | /bin/sh -c " ara-manage runserver 0.0.0.0:8032"        |
| portforwardingf       | /bin/sh -c " socat tcp-listen:8034,reuseaddr,fork tcp:localhost:9000"        |
| cronicle       | /bin/sh -c "cd $CRONICLE_DIR;  rm ./logs/cronicled.pid || true; . env/bin/activate; $CRONICLE_DIR/bin/control.sh setup; $CRONICLE_DIR/bin/control.sh start"        |
| staticfileserve       | /bin/sh -c "cd $STATIC_FILESERVER_DIR;  . env/bin/activate; serve -p 8028 /home/static-server"        |
| codeserver       | /bin/sh -c " code-server --bind-addr 0.0.0.0:8021 --auth "none" --disable-telemetry /home"        |
| filebrowser       | /bin/sh -c " /home/abc/apps/filebrowser/filebrowser -c /home/abc/apps/filebrowser/.filebrowser.json"        |
| ungit       | /bin/sh -c "cd $HOME/apps/ungit;  . env/bin/activate; ungit --port=8024 --ungitBindIp=0.0.0.0 --launchBrowser=false --autoFetch=false --bugtracking=false --authentication=false"        |
| htop       | /bin/sh -c "export TERM=xterm;  ttyd -p 8027 /bin/zsh -c '/usr/bin/htop'"        |
| mc       | /bin/sh -c "export TERM=xterm; export EDITOR=mcedit;  ttyd -p 8026 /bin/zsh -c '/usr/bin/mc'"        |
| admin       | /bin/sh -c "export TERM=xterm;  ttyd -p 8025 /bin/zsh -c 'alnoda-wrk admin'"        |
