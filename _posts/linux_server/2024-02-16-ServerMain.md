---
title: "조립부터 시작하는 리눅스서버 생활"
comments: true
categories: linux-server
tags:
  - linux
  - server
---

! 이 내용은 notion에서 작성한 것을 옮긴것입니다. 사진 순서가 이상하시거나 내용 순서가 꼬인것이 있으면 [이곳](https://justyolo.notion.site/9765130d23254868b56e6e5ced441da8?pvs=4)을 참조해주세요.

# 조립부터 시작하는 리눅스서버 생활

원래는 시놀로지를 사려고 했는데… 가격이 비쌉니다.

![소프트웨어 값이 대부분인 시놀로지…](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/bb755a33-ded6-4414-99e4-65e41335382b)

소프트웨어 값이 대부분인 시놀로지…

플러스 아닌 모델(224, 220)도 꽤 비싸더군요…

사실은 헤놀로지(XPEnology)를 쓸까 했는데, 라이선스 문제도 있어서 이왕김에 하드웨어 구성부터 시작해서 새로운 리눅스 서버를 만들려고 합니다.

일단 목표는: 

- [x]  Docker 사용하기
- [x]  RAID 구성으로 NAS 서버 구축하기 (내부망: SMB, 외부망: WebDAV & Mac TimeMachine)
- [ ]  Spring 올리기
- [ ]  Discord 봇 올리기
- [ ]  Java 마인크래프트 서버 올리기 (Docker)

정도가 될것 같습니다. 

하드웨어 스펙은 (2023년 12월 21일 최초 구성 당시 기준):

**CPU: Intel® Pentium® Gold G5400 Processor (8세대 Coffee Lake)**

**M/B: ASRock H310CM-DVS**

**RAM: DDR4-2400 4GB * 2 (듀얼 채널)**

**~~VGA: RTX3070~~ (Ubuntu 설치시에만 잠깐 사용)**

**SSD: Plextor 120GB ??** 

**HDD: Seagate IronWolf 4TB * 2 (ST4000VNZ06)(RAID 구성 예정)**

**POWER:** **SuperFlower SF-650F14MT WHITE LEADEX SILVER**

**CASE: ANIX M6509 MINI**

(파워는 남은거 사용 했습니다만… 메인컴퓨터가 게임 하면서 자꾸 꺼지는 원인이 이 친구 같아서 교환한건데… idle일때는 괜찮아서 두고봐야겠네요)

CPU + M/B + RAM은 택포 8만원으로 중고 구매

HDD는 블프때 개당 10만원 안쪽으로 구매 ~~(관세랑 배송비가 있지만…)~~

케이스도 3만원 정도에 구입

일단 시놀로지도 하드 제외하고 30~50만원 정도 하니 매우 저렴하게 만들었는데, 어떻게 될지 기대되네요.

- 목록
    
    [리눅스 서버만들기 - 1일차](../Server1)
    
    서버 누드 테스트 및 Ubuntu 설치 및 세팅
    
    [리눅스 서버만들기 - 2일차](../Server2)
    
    서버 조립 및 DHCP 자동 설정
    
    [리눅스 서버만들기 - 3일차](../Server3)
    
    RAID 구성, SMB, WebDAV, AFP 등 (Apple Time Machine)
    
    [리눅스 서버만들기 - 4일차](../Server4)
    
    mount, umount 및 서버 재부팅시 RAID 재구성 문제
    
    [리눅스 서버만들기 - 5일차](../Server5)
    
    공유기 VPN 설정 ~~실패~~ 및 Docker 입문
    
    [리눅스 서버만들기 - 6일차](../Server6)
    
    공유기 VPN 설정 (서브넷 변경) 및 Docker에 Netdata 올리기 
    
    [리눅스 서버만들기 - 7일차](../Server7)
    
    HDD/SSD 벤치 및 Tailscale 설정
    

[마인크래프트 서버 만들기 (진행중)](../ServerMine)

[Terrapin (**CVE-2023-48795)**](../Server-Terrapin)

도움을 받은 곳:

[https://svrforum.com/nas/573376](https://svrforum.com/nas/573376) 하드웨어 스펙

[https://svrforum.com/software/1129530](https://svrforum.com/software/1129530) VPN 접속 관련 (서브넷 문제)

예?정:

 [하드 핫스왑베이](https://ko.aliexpress.com/item/1005004679846265.html?srcSns=sns_Copy&spreadType=socialShare&bizType=ProductDetail&social_params=21209143504&tt=MG&shareId=21209143504&businessType=ProductDetail&platform=AE&afSmartRedirect=y)

구매한 케이스에 요즘 보기 흔치않은 5.25인치 베이 (ODD)가 2칸이 있어서 이런게 들어가면 좋을것 같은데… ~~중국산 치곤 가격이…~~

2023.12.23 update: 케이스를 받아보니 5.25인치 베이랑 파워랑 간섭이 있어서 베이를 사용하는건 어려울것 같네요… 꼭 필요하면 파워를 바꾸던지 해야겠습니다.

#### PCIE std → m.2 nvme 변환카드 (HDD 캐싱용)

원래는 하드디스크 캐싱이 필요 없을것 같았고 메인보드에 m.2 nvme 슬롯도 없어서 SSD가 추가로 필요 없을줄 알았는데, 생각보다 하드디스크의 속도가 느리고 PCIE 포트가 놀고있으니 괜히 SSD 사서 연결하고 싶은 생각이 듭니다. 돈이 남아 돌거나(?) 꼭 필요하다고 생각되면 추가하도록 하겠습니다.