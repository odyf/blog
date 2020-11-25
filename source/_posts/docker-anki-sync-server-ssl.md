---
title: docker-anki-sync-server-ssl
tags:
  - docker-compose
  - docker
  - ssl
abbrlink: 21678
date: 2020-11-24 08:21:45
---

```
version: "3"

services:
    anki-container:
        image: kuklinistvan/anki-sync-server:latest
        container_name: anki
        restart: always        
    environment:
       - TZ=Asia/Shanghai
        volumes:
        - ./data:/app/data

        
```

