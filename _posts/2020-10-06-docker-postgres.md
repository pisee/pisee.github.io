---
title: docker-postgres
permalink: /docker-postgres/
---

# Docker Postgres 설치
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

# 컨테이너 접속/database,user 생성
```bash
$ docker exec -it some-postgres bash

bash-5.0# psql -U postgres
psql (9.6.19)
Type "help" for help.

postgres=# CREATE USER doni WITH PASSWORD 'doni';
CREATE ROLE
postgres=# CREATE DATABASE doni;
CREATE DATABASE
postgres=# GRANT ALL PRIVILEGES ON DATABASE doni TO doni;
GRANT
postgres=# CREATE DATABASE [DB명] WITH OWNER [user명] ENCODING 'UTF-8' TEMPLATE template0;
postgres=# \l
postgres=# \q
bash-5.0# exit
```
```bash

```

# 컨테이너 접속/ 데이터베이스 접속설정 수정
postgres sample 은 ```/usr/local/share/postgresql```
```bash
$ docker exec -it some-postgres bash
bash-5.0# cd /var/lib/postgresql/data

bash-5.0# vi pg_hba.conf

#IPv4
host    all    all    127.0.0.1/32    trust   # --> 
host    all    all    0.0.0.0/0       password  #로 수정 저장

bash-5.0# vi postgresql.conf

listen_addressed = '*'
port = 5432 
# 수정 저장

bash-5.0# exit

# 도커 재시작
$ docker restart some-postgres
```

