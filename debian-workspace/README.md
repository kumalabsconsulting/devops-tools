# Debian-Workspace

L'idée est d'avoir une image de base sous debian stable avec un seul utilisateur **abc**.

Il n'a que les droits sudo sur le gestionnaire apt.

## Push to Docker Hub
```bash
docker login
docker push redbeard28/debian-workspace:TAG_NAME
```


## Pull from Docker Hub
```bash
docker pull redbeard28/debian-workspace:TAG_NAME
```

## Inspiration

[__alnoda-wrk__](https://pypi.org/project/alnoda-wrk/) installed, and workspace initialised. 