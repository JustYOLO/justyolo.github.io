---
title: "lvm 시스템 볼륨 확장 (live)"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - partition
  - lvm
---

개인 우분투 서버에 디스크 사용량이 90%에 도달했습니다.

![fd-h](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/a03e69a7-c82a-46d6-bc0c-64685758042b)

![lsblk](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/4be97030-1555-4c0a-8cd1-d51e53899ff2)

fd -h와 lsblk로 확인해본 결과입니다.

저는 시스템 볼륨이 lv으로 만들어진지 몰라서 마운트를 해제하고 크기를 늘리는 방법을 찾고 있었는데, lv (logical volume)으로 만든 볼륨은 라이브로 늘릴 수 있다고 합니다.

![vgdis](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/968bc3b6-b88d-4f3f-be4a-6dfa0ea293c8)

vgdisplay로 해당 ubuntu를 설치한 볼륨이 lvm2 format으로 되어있고, 116GiB중 58GiB만 할당된 것을 볼 수 있습니다.

![lvextend](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/fba6206d-d749-4e04-a573-32cb825af457)

lvextend로 볼륨을 확장한 모습입니다.
> $ sudo lvextend -L+10G (lvm target)

일단 10GiB만 늘려봤습니다.

이렇게 늘리면 논리 볼륨 사이즈만 늘린것이고, file system까지 늘려야합니다. ext4, btrfs같은 파일 시스템은 live로 사이즈를 늘리는 것을 지원한다고 합니다. (shrinking는 별개)

![resize2fs](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/7a937501-5e12-4bd4-878e-00bfda1b9fd8)

>$ resize2fs (lvm target)

로 확장한 논리 볼륨 사이즈만큼 자동으로 file system 사이즈를 늘리고, df -Th로 확인한 모습입니다.

[link](https://unix.stackexchange.com/questions/58432/must-the-filesystem-be-unmounted-while-extending-an-lvm-logical-volume)
