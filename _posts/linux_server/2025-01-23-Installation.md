---
title: "Ubuntu 설치시 발생하는 문제 정리"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - ssh
  - xrandr
  - dns
---

이번에 서버를 옮기는 작업을 하면서 Ubuntu를 재설치하는 과정중에 마주했던 여러 문제를 정리해봤습니다.

## 화면 잘림

현재 서버랙에 있는 모니터가 4:3 비율 모니터입니다. 그런데 우분투 설치 디스크 (22.04 Desktop version)로 부팅했을때 화면이 잘려서 출력되어서 설치를 진행할 수 없었습니다. 
그래서 xrandr 명령어를 사용해서 화면 비율을 바꿔서 설치를 진행했습니다.
설치 화면에서 ctrl + alt + t로 터미널을 띄울 수 있습니다. (Ubuntu Desktop과 동일)

```bash
$ xrandr
```

을 실행하면 현재 해상도 정보가 나옵니다.

예시)

```
Screen 0: minimum 320 x 200, current 3200 x 1080, maximum 8192 x 8192 
VGA-1 disconnected (normal left inverted right x axis y axis)
HDMI-1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 531mm x 299mm
   1920x1080     59.93 +  60.00*   50.00    59.94  
   1920x1080i    60.00    50.00    59.94  
   1680x1050     59.88  
```

여기에서 해당하는 해상도로 변경해주시면 됩니다.

```bash
$ xrandr --output (장치명) --mode (해상도)
```

위에서 장치명은 HDMI-1, 해상도는 1920x1080과 같이 출력된 형식만 사용이 가능한것 같습니다.


## Ping은 가지만 apt 등 도메인 서비스가 작동하지 않음

설치 이후에 apt를 사용하려고 했는데 서버 연결이 안되더군요. 그런데 ping 8.8.8.8 하면 응답은 정상적으로 왔습니다.
그래서 GUI를 이용해서 고정 IP 설정하는 곳에서 DNS를 지정해줬는데도 DNS를 사용하지 않았습니다.

다른 방법으로 /etc/resolv.conf 파일을 수정해서 DNS를 추가해줬습니다.

vi로 열어서 주석 처리된 부분 이후에 (# 으로 시작하는)

```
nameserver 8.8.8.8
```

로 추가해주면 도메인을 사용하는 서비스들이 정상 작동합니다.