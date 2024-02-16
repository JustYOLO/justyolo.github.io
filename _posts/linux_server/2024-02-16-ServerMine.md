---
title: "리눅스 서버에 마인크래프트 서버 올리기 (docker)"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - minecraft
  - docker
---
# 마인크래프트 서버 만들기 (진행중)

마인크래프트 서버를 만들어 보려고 합니다.

참조: [https://www.llewellynhughes.co.uk/post/installing-minecraft/](https://www.llewellynhughes.co.uk/post/installing-minecraft/)

[https://docker-minecraft-server.readthedocs.io/en/latest/](https://docker-minecraft-server.readthedocs.io/en/latest/)

먼저 docker container에 필요한 폴더를 생성해줍니다:

![Screenshot 2024-01-01 at 11.44.45 PM.png](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/e3425a84-26d3-4a2f-8aaf-bffc5b4db39e)

docker-compose.yml 파일을 만들어서:

```
version: "3.8"

services:
  mc:
    image: itzg/minecraft-server
    tty: true
    stdin_open: true
    ports:
      - "25565:25565"
    environment:
      EULA: "TRUE"
      SPAWN_MONSTERS: "TRUE"
      MODE: "SURVIVAL"
      SERVER_NAME: "TESTSERVER"
    volumes:
      - ./data:/data
```

내부 내용을 원하는 대로 바꾸고 저장해줍니다.

이후 docker-compose.yml이 있는 폴더에서

```bash
docker-compose up -d
```

을 실행하면 됩니다.

rcon (마인크래프트 내부 명령어 실행)도 사용 가능합니다.

```bash
sudo docker exec -i (id) rcon-cli
```

명령어로 rcon-cli를 사용 할 수 있습니다. 저는 화이트리스트 추가로 사용하고 있습니다.

1일차:

**Can't keep up! Is the server overloaded? 오류**

[https://jizard.tistory.com/321](https://jizard.tistory.com/321)

에서 관련 내용을 찾았습니다.

docker-compose.yml 파일에

```bash
environment:
      EULA: "TRUE"
      SPAWN_MONSTERS: "TRUE"
      MODE: "SURVIVAL"
...
      JVM_OPTS: "-XX:+IgnoreUnrecognizedVMOptions -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:InitiatingHeapOccupancyPercent=15 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true"
```

이런식으로 추가 했습니다.