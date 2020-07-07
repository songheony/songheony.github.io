---
layout: post
title: Ubuntu apt update에서 404 Not Found가 나올시
comments: true
---

### apt-get update

가끔 우분투에서 apt update 혹은 apt-get update를 실행할 시  
404 Not Found 에러를 만날 경우가 있다.  
원인은 여러가지가 있는데, 그중 가장 많은 사람들이 겪는 원인은  
사용하는 우분투 버전이 더이상 지원이 되지 않는 버전인 경우이다.  
이런경우 가장 좋은 방법은 가장 최신의 LTS를 설치하는 것 이지만,  
불가피하게 포멧을 못할시에는 아래와 같이 수정을 하면 된다.

### 업데이트 저장소 변경

우선 아래의 명령어로 저장소 주소가 저장되어 있는 파일을 연다.

```sh
$ sudo gedit /etc/apt/sources.list
```

그후, `deb http://xx.archive.ubuntu.com` 라고 되어 있는곳을 전부 `deb http://old-releases.ubuntu.com` 로 바꿔준다.  
마찬가지로 `deb http://security.ubuntu.com` 라고 되어 있는곳을 전부 `deb http://old-releases.ubuntu.com` 로 바꿔준다.  
단 xx는 본인의 기존 저장소 위치이다. 예를들어 국가코드(kr, jp 등)이 될 수도 있고, 특정 회사의 것일 수도 있다.  

다 변경후 저장한뒤

```sh
$ sudo apt update
```

를 진행하면 제대로 되는것을 볼 수 있을것이다.  
혹시나 이것으로 해결되지 않는다면, 다른 원인인 가능성이 높으므로 다른에러를 찾는것이 좋을것이다.