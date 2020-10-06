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

# Docker volume 생성
```bash
$ docker volume create postgres
$ docker volume ls
4 docker volum inspect postgres
```

# postgres instance 시작
```bash
$ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres:9.6.19-alpine
$ docker run -p 5432:5432 --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -v postgres:/var/lib/postgresql/data -d postgres:9.6.19-alpine
```
- -name: 인스턴스명
- -e : 컨테이너에 환경변수 전달
- POSTGRES_PASSWORD
- -d : 데몬모드로 실행
- -v 볼륨연결
- 도커 컨테이너 확인
```bash
$ docker ps -a
```

# 컨테이너 접속
```bash
$ docker exec -it some-postgres bash

bash-5.0# psql -U postgres
psql (9.6.19)
Type "help" for help.

postgres=#
```


