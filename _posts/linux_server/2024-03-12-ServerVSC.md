---
title: "Ubuntu server에서 VSC Server 사용하기"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - VSC
  - VSCode
  - code
---

# Ubuntu server에서 VSC server 사용하기

원래 서버에서 코딩할 생각이 없었는데 이번에 리눅스 커널에서 printk로 dmesg를 띄우는 과제를 진행하다 리눅스 환경에서 vscode로 진행하는게 좋을 것 같아서 remote 환경을 구성해보려합니다.

## snap으로 vscode 설치

원래는 apt로 설치하려고 했는데 없어서 검색해보니:
```bash
sudo snap install --classic code
```
로 vscode를 설치할 수 있었습니다.

이후 똑같이 shell에 

```bash
code tunnel 
```
로 vscode를 실행시키면 github이나 microsoft 계정으로 인증을 진행하라고 링크를 제공합니다.

![remote explorer](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/a12fce1c-6001-46ce-a7d7-8eca494b6954)

링크를 복사해서 인증하면 웹사이트에서 접속하거나, 같은 계정으로 로그인한 vscode에서 remote explorer에서 접속할 수 있습니다.
