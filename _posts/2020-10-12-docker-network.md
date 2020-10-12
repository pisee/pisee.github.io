---
title: docker-network
permalink: /docker-network/
---

# Docker Network 명령어
```bash
$ docker network ls
$ docker network create my-net
$ docker network rm my-net
```

# Docker Network 연결/해제
```
$ docker network inspect my-net
$ docker network connect my-net [container-name]
$ docker network disconnect my-net [container-name]
```
