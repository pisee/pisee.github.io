---
title: docker-postgres
permalink: /docker-postgres/
---

# Docker postgres 설치
- 도커허브에서 버전조회  
https://hub.docker.com/_/postgres?tab=tags  

- Dockerfile 참고  
https://github.com/docker-library/postgres  

# 설치
```
$ docker pull postgres:9.6.19-alpine
```

# postgres instance 시작
```bash
$ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```
- name: 인스턴스명
- POSTGRES_PASSWORD

- 도커 컨테이너 확인
```bash
$ docker ps -a
```

# 
