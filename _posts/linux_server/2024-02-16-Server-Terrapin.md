---
title: "Terrapin(CVE-2023-48795) 취약점"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - cve
  - terrapin
  - security
---

# Terrapin (CVE-2023-48795)

[https://jfrog.com/blog/ssh-protocol-flaw-terrapin-attack-cve-2023-48795-all-you-need-to-know/#how-cve-2023-48795-was-fixed-in-openssh](https://jfrog.com/blog/ssh-protocol-flaw-terrapin-attack-cve-2023-48795-all-you-need-to-know/#how-cve-2023-48795-was-fixed-in-openssh)

Chrome 모바일에서 이런 기사를 제공해서 알게 되었습니다. 혼자 사용한다해도 보안은 중요한 사항이기 때문에 수정하도록 하겠습니다.

Terrapin 취약점을 스캔할 수 있는 스캐너를 제공하고 있습니다:

[https://github.com/RUB-NDS/Terrapin-Scanner/releases/tag/v1.1.0?_gl=1*1gdqm9h*_ga*MjEyNTM4OTYwOS4xNzA0MjcwMTYx*_ga_SQ1NR9VTFJ*MTcwNDI3MDE2MS4xLjEuMTcwNDI3MDY5OC4wLjAuMA](https://github.com/RUB-NDS/Terrapin-Scanner/releases/tag/v1.1.0?_gl=1*1gdqm9h*_ga*MjEyNTM4OTYwOS4xNzA0MjcwMTYx*_ga_SQ1NR9VTFJ*MTcwNDI3MDE2MS4xLjEuMTcwNDI3MDY5OC4wLjAuMA)

취약점은 server와 client 모두 해결해야함으로 각각 따로 체크하겠습니다.

먼저 client인 macbook air부터 보겠습니다.

위 레포에서 macos_arm64 버전을 받아서 체크하겠습니다.

받아서 chmod +x 로 실행 가능하게 바꾼뒤 실행하겠습니다.

```jsx
./Terrapin_Scanner_MacOS_arm64_darwin --listen 8888
```

로 특정 포트를 listen하게 만들고,

다른 터미널로 접속하시면 됩니다.

![Screenshot 2024-01-03 at 6.09.25 PM.png](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/f7b585f9-16da-4d9d-bc04-15e68d389936)

결과는 이렇게 됩니다.

패치 방법은 /etc/ssh/ssh_config 나 /etc/ssh/sshd_config 파일에

```jsx
Ciphers -chacha20-poly1305@openssh.com
```

를 추가하면 됩니다.

![Screenshot 2024-01-03 at 6.15.07 PM.png](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/4555046e-8ae4-449f-b811-1b125894dd04)

수정후엔 이렇게 뜹니다.

다음에는 제 리눅스 서버입니다.

![Screenshot 2024-01-03 at 6.16.45 PM.png](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/05967e34-2a7e-4757-a2cd-e2af932b3067)

다행히도 서버는 괜찮은것 같습니다.