---
layout: post
title: Docker에 Matlab 설치하기
---

### Docker에 Matlab을 설치하는 이유?

제가 있는 연구실에서는 GPU작업을 위한 워크스테이션이 설치되어 있고,  
워크스테이션에서 작업을 하기 위해서는 각자의 Docker환경을 설치하고 그 위에서만 스크립트를 실행해야 합니다.  
이럴때 가끔 Matlab을 사용해야할 떄는 정말 힘들다보니 Docker에 Matlab을 설치하는 방법을 찾게 되었습니다.  


### CUI? GUI?

첫번째로 검색해서 나온 내용은 GUI을 이용한 Matlab 설치방법 이였습니다.  
방법은 생각보다 간단해서, Docker환경의 Ubuntu에 데스크탑 환경을 설치하고  
ssh을 이용해서 GUI로 접속해서 Matlab을 설치하는 것이였습니다.  
하지만 이 방법을 위해서는 수동으로 GUI환경을 계속 만줘져야 하는게 귀찮고,  
OS가 달라지면 그에 맞게 설정을 따로 해줘야해서 힘들었습니다.  

### CUI로 설치하는 방법

제가 사용한 방법은 Matlab을 설치할때 installer_input.txt을 이용해서 각종 설정을 변경해준것입니다.  
Matlab의 경우 설치할때 -inputFile 옵션으로 설정파일을 넘길 수 있었습니다.  
그리고 대부분의 Matlab 패키지의 경우, 설치폴더를 찾아보시면 installer_input.txt을 찾으실 수 있습니다.  
installer_input.txt에서 설정해야 하는곳은  

* fileInstallationKey
* agreeToLicense
* mode
* licensePath

입니다.  
fileInstallationKey와 licensePath의 경우는 각자의 라이센스 코드와 라인센스 파일(license.data)의 경로를 설정해주시면 됩니다.  
저 같은 경우는, 설치폴더안에 license.data파일을 넣어놓고  

```bash
licensePath=/R2019a/license.data
```

와 같이 설정해주었습니다(R2019a버전 기준).  

그외에는 다음과 같이 설정해주시면 됩니다.

```bash
agreeToLicense=yes
mode=silent
```

이상으로 설정파일의 변경은 끝입니다.

### Dockerfile에서는

Dockerfile에서는 기존의 Dockerfile의 가장 아래에

```sh
COPY R2019a /R2019a
RUN /R2019a/install -inputFile /R2019a/installer_input.txt
RUN ln -s /usr/local/R2019a/bin/matlab /usr/local/bin/matlab
RUN rm -rf /R2019a
```

와 같이 써주신후 빌드하시면 됩니다.  
위의 코드는 어디까지나, Ubuntu환경과 R2019a버전을 기준으로 작성하였습니다.