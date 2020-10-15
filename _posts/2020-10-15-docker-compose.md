---
title: docker-compose
permalink: /docker-compose/
---

# Docker-compose 설치
https://docs.docker.com/compose/install/  
```bash
$ com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ cd /usr/local/bin/docker-compose
$ chmod 775 docker-compose
$ docker-compose --version
```

# 설정
https://docs.docker.com/compose/gettingstarted/  
```docker-compse.yml```생성
```yml
version: '3'

services:
  db:
    image: postgres
    volumes:
      - ./docker/data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=doni
      - POSTGRES_USER=doni
      - POSTGRES_PASSWORD=doni
      - POSTGRES_INITDB_ARGS=--encoding=UTF-8

  django:
    build:
      context: .
      dockerfile: ./compose/django/Dockerfile-dev
    environment:
      - DJANGO_DEBUG=True
      - DJANGO_DB_HOST=db
      - DJANGO_DB_PORT=5432
      - DJANGO_DB_NAME=sampledb
      - DJANGO_DB_USERNAME=sampleuser
      - DJANGO_DB_PASSWORD=samplesecret
      - DJANGO_SECRET_KEY=dev_secret_key
    ports:
      - "8000:8000"
    command: 
      - python manage.py runserver 0:8000
    volumes:
      - ./:/app/
```
