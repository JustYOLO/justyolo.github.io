---
title: "scp로 맥에서 리눅스 서버로 파일 전송"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - transfer
  - tailscale
  - scp
---

맥에서 리눅스 서버로 파일을 전송할 경우가 자주 생겼습니다. 
처음에는 네트워크 폴더에 옮기는 방법을 주로 사용했었는데, scp를 알게되어서 사용하게 되었습니다.

scp는 Secure Copy Protocol로, SSH를 기반으로 제작된 프로그램입니다. 따라서, 호스트와 클라이언트가 모두 ssh를 사용하면 scp를 사용할 수 있을 것 같습니다.

간단하게, 
```shell
scp -P 1111 ./testfile root@lab
```

이런식으로, -P는 포트번호 설정 옵션입니다.
받는 곳의 dir를 설정하지 않으면 계정의 home dir에 전송됩니다.

반대로,
```shell
scp -P 1111 root@lab:/testfile ~/testfile
```

이렇게 사용하면 서버에 있는 데이터를 가져올 수 있습니다.