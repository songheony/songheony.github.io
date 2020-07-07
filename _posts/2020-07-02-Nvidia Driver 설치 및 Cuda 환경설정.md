---
layout: post
title: Nvidia Driver 설치 및 Cuda 환경설정
comments: true
---

### 설치하기에 앞서

기본적으로 이글은 딥러닝을 하고 싶어하며, 편하게 설정하고 싶어하는 사람들을 대상으로 합니다.  
특정 드라이버를 사용해야만 하는 분들은 다른 글들을 찾아보시는게 편합니다.  
또한, OS는 우분투를 기준으로 합니다.

### Nvidia Driver 설치

혹시 nvidia-driver가 기존에 설치되어 있으신 분들은 아래의 코맨드로 삭제하시기 바랍니다.

```sh
$ sudo apt --purge autoremove nvidia*
```

Nvidia Driver를 설치하는 가장 편리한 방법은 최신의 버전을 OS가 추천해주는대로 쓴다 입니다. 

```sh
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo ubuntu-drivers autoinstall
$ sudo reboot
```

### Cuda 환경설정

여러분이 Cuda를 사용하는 주된 이유는 아마 tensorflow나 pytorch같은 유명한 딥러닝 라이브러리를 이용하기 위함이라고 생각합니다.  
하지만, 여러분이 아셨으면 하는것은 [Anaconda](https://anaconda.org)를 이용하시면 Cuda와 cudnn의 설치없이 편하게 이용할 수 있다는 사실입니다.  
특히나 [tensorflow](https://anaconda.org/anaconda/tensorflow-gpu)와 [pytorch](https://anaconda.org/pytorch/pytorch)는 최신의 거의 모든 버전이 추가설치 없이 사용할 수 있도록 빌드되어져 있기 때문에, 가장 추천하는 방법은 Anaconda를 이용한 가상환경에서 작업하는것입니다.  

하지만 정말 불가피한 사정으로 Cuda를 설치해야 하는분들을 위해 아래에 방법을 적어놓습니다.

먼저 원하시는 Cuda를 Nvidia 홈페이지로부터 다운받습니다.  
이후의 설명은 [Cuda 9.2](https://developer.nvidia.com/cuda-92-download-archive)를 기준으로 합니다.  
`Installer Type`을 `runfile(local)`을 선택하셔서 파일을 다운로드 합니다.  

```sh
$ sudo sh ~~.run
```

위의 코드를 이용하여 설치파일을 실행시킨후, 아래의 순서대로 입력해줍니다.

```sh
Do you accept the previously read EULA?
accept/decline/quit: accept

You are attempting to install on an unsupported configuration. Do you wish to continue?
(y)es/(n)o [ default is no ]: y

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 xxxxx?
(y)es/(n)o/(q)uit: n

Install the CUDA 9.2 Toolkit?
(y)es/(n)o/(q)uit: y

Enter Toolkit Location
[ default is /usr/local/cuda-9.2 ]:

Do you want to install a symbolic link at /usr/local/cuda?
(y)es/(n)o/(q)uit: n

Install the CUDA 9.2 Samples?
(y)es/(n)o/(q)uit: n
```

혹시나 컴파일에 문제가 있으신분은 gcc버전이 맞지 않음을 의심해보셔야 합니다.  
대표적으로 Cuda 9.2는 gcc6을 필요로 합니다.  
아래의 코드로 gcc를 설치해준후 다시 Cuda의 설치를 진행합니다.  

```sh
$ sudo apt install gcc-6 g++-6
$ sudo unlink /usr/local/bin/gcc
$ sudo unlink /usr/local/bin/g++
$ sudo ln -s /usr/bin/gcc-6 /usr/local/bin/gcc
$ sudo ln -s /usr/bin/g++-6 /usr/local/bin/g++
```

마지막으로 Cuda가 제대로 설치되었는지 확인합니다.

```sh
$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Tue_Jun_12_23:07:04_CDT_2018
Cuda compilation tools, release 9.2, V9.2.148
```

혹시 위와같이 `Cuda compilation tools, release 9.2` 라고 출력되지 않는다면 기존의 다른 Cuda가 설치되어 있을수도 있습니다.  
Cuda는 한 환경에 여러개가 설치되어도 무방합니다. 다만 편한사용을 위해서 `/usr/local/cuda` 폴더에 링크를 설치하는것으로 버전을 바꿔가면서 사용합니다.

```sh
$ sudo unlink /usr/local/cuda
$ sudo ln -s /usr/local/cuda-9.2 /usr/local/cuda
```

마지막으로 우분투의 환경변수를 바꿔줍니다.

```sh
$ gedit ~/.bashrc
```

명령어를 실행시키면 노트가 하나 나오는데, `/usr/local/cuda-xx` 라고 설정되어 있는곳을 `/usr/local/cuda` 로 수정해줍니다(xx는 임의의 버전입니다. 예를들어 9.2가 될 수 있습니다.).

```sh
$ source ~/.bashrc
```

위의 명령어를 통해 환경변수를 적용시켜줍니다.