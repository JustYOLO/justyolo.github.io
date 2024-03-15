---
title: "ASLR(Address Space Layout Randomization)"
comments: true
categories: linux-server
tags:
  - linux
  - server
  - ASLR
  - address
---

# ASLR

학부 운영체제 수업시간에 C언어로 malloc()을 사용하는 프로그램을 동시에 두개 실행하는 예제가 있었습니다:
```c
// mem.c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include "common.h"

int main(int argc, char *argv[]) {
    if (argc != 2) { 
	fprintf(stderr, "usage: mem <value>\n"); 
	exit(1); 
    } 
    int *p; 
    p = malloc(sizeof(int));
    assert(p != NULL);
    printf("(%d) addr pointed to by p: %p\n", (int) getpid(), p);
    *p = atoi(argv[1]); // assign value to addr stored in p
    while (1) {
	Spin(1);
	*p = *p + 1;
	printf("(%d) value of p: %d\n", getpid(), *p);
    }
    return 0;
}
```
[출처 - OSTEP](https://github.com/remzi-arpacidusseau/ostep-code/blob/master/intro/mem.c)

```bash
./mem & ./mem &
```

이렇게 같은 프로그램 2개를 백그라운드 프로세스로 실행시키면:

![그림1](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/0bb2dce3-1ff7-4efd-bf2e-b8b8a3fc0cf9)

다른 주소가 출력됩니다.
OSTEP의 예제에서는 같은 주소가 나온다고 되어있어서 이유를 찾아본 결과 stack randomizer, Address Space Layout Randomization(ASLR)을 사용하기 때문인것으로 확인했습니다. 
(처음에는 난독화 관련인줄 알고 gcc -O0 같은 옵션을 사용했는데 메모리 주소는 같게 나오지 않았습니다.)

 ASLR은 프로세스의 주소 공간의 무작위로 배치 함으로서 return-to-libc 공격과 같은 메모리 오염 취약점(memory corruption vulnerabilities)을 방지하는 기술입니다. 메모리 segmentation의 stack에는 local 함수의 return address가 존재합니다. 이 주소를 알 수 있다면 다른 값을 집어넣어 code injection을 할수 있게됩니다. 

 linux에서는 (적어도 제 서버에서는)
```bash
/proc/sys/kernel/randomize_va_space
```
에 설정값이 있는데 기본값은 2이고, 0으로 설정하면 ASLR을 적용하지 않게됩니다. (변경후 reboot 필요)

![2](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/72c4057b-6c0e-4dfa-9caf-ebb4fc07d403)

이렇게 변경해주고 재부팅후 mem 프로그램을 실행시키면:
![3](https://github.com/JustYOLO/justyolo.github.io/assets/31424495/446bb448-a926-44d9-8282-c387eb45e51c)

가상 메모리 주소가 같게 나오는 것을 볼 수 있습니다.


[참조: ASLR의 wiki](https://en.wikipedia.org/wiki/Address_space_layout_randomization)

[참조2: Return to libc attack의 wiki](https://en.wikipedia.org/wiki/Return-to-libc_attack)