---
title: "neovim 'gd'로 code navigation (compile_commands.json 연결)"
comments: true
categories: research
tags:
  - linux
  - server
  - leveldb
  - neovim
  - vim
  - lsp
---

최근 연구실 선배님이 neovim 사용법을 알려주시면서 vscode대신 neovim을 사용하는 연습을 하고있습니다.
진입장벽이 높은 neovim인 만큼, 기초적인 vim command 이외에는 매번 검색해야 할 정도로 복잡했습니다.

몇가지 아쉬운 점이 있었는데, (~~visual mode yank시 system clipboard에 연동이 안되는 점~~ 해결) 그 중 lsp로 "gd"를 사용해서 코드 정의 부분으로 넘어갈때 제대로 동작하지 않았다는 점 입니다. 

검색하다 알아본 결과, compile_commands.json이라는 파일이 있어야 정상적으로 동작한다는 것을 알았습니다. 이는 cmake에서 build시 자동으로 만들어 주도록 설정할 수 있습니다.

CMakeLists.txt에서 적당한 부분에 

```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```
를 추가해주고 build 해주시면, build 디렉토리에 compile_commands.json가 생긴것을 보실 수 있습니다. 이를 ln으로 소스코드 최상위 dir에 링크해두시면 gd가 정상적으로 동작하는 것을 확인할 수 있습니다.

~~매번 잊어먹어서 글을 썼습니다...~~