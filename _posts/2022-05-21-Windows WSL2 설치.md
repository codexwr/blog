---
title: Windows WSL2 설치
date: 2022-05-21 12:10:00 +0900
categories: [windows, system]
tags: [windows, wsl]
toc: true
comments: true
---

윈도우에 WSL2를 설치하기 위해 검색해 보면 많은 블로그의 글들에서 dism을 이용하여 커널 및 우분투 설치를 진행하는 복잡한 설치 방법을 소개하고 있다.  
하지만 최근의 wsl 설치 방법은 명령어 한줄이면 끝난다. 심지어 짧다!!

## 사전 준비 사항

특별한건 없다. OS 버전만 최소 요구사항을 맞추면 된다.

> Windows 10 version2004 이상 (Build 19041 이상) 혹은 Windows 11

## 설치

아래 명령어로 설치를 진행한다. 기본값은 WSL2와 우분투가 설치된다.

- **`관리자 권한`**의 콘솔에서 아래 명령어를 수행한다.
  ```console
  wsl --install
  ```
- 설치가 완료된 이후 시스템을 재부팅하면 된다.
  > **Note:** 더 다양한 설치 옵션은 [Microsoft 문서](https://docs.microsoft.com/ko-kr/windows/wsl/install)를 참고하자.
  {: .prompt-info }

## 부가 옵션

wsl에서는 chmod, chown 명령어가 적용되지 않는다. 해당 문제는 아래 두가지 방법중 편한걸로 사용하자.

- wsl 설정 파일  
  `/etc/ws.conf`{: .filepath}에 아래의 내용을 추가한다.
  ```ini
  [automount]
  options = "metadata"
  ```
  {: file='/etc/wsl.conf'}

  - 설정후 wsl를 재시작한다. wsl 명령은 [Microsoft 문서](https://docs.microsoft.com/ko-kr/windows/wsl/basic-commands)를 참조한다.

  > wsl 설정에 관련된 더 자세한 사항은 [Microsoft 문서](https://docs.microsoft.com/ko-kr/windows/wsl/wsl-config)를 참조한다.
  {: .prompt-info }

**OR**

- 마운트 재설정
  ```console
  $ sudo umount /mnt/c
  $ sudo mount -t drvfs C: /mnt/c -o metadata
  ```

<br/><br/>

## 잡설

개발머신은 항상 Mac을 사용해 왔다. 그런데 이번에 어쩌다보니 Windows PC를 지급받게 되었다.  
평소 Mac에서 로컬 개발환경을 꾸민 것처럼 Windows에도 꾸미고 싶었다. zsh, docker, sdk 버전 관리 툴(nvm, jenv) 등 셋팅을 위해서 WSL2를 사용하기로 하였다.

- 평소에 Mac을 쓰는데 불편함이 없었고 docker 환경을 자주 이용한다면 그냥 Mac을 써라고 말하고 싶다.  
  볼륨을 생성(_docker volume create_)해서 사용하는 것에서는 크게 문제가 없었는데, 로컬 경로를 볼륨 마운트해서 사용하는 경우(_예를 들면 docker run -v /mnt/c/test:/tmp/test_)에는 컨테이너를 stop후 start 할 경우 간헐적으로 mount 오류가 발생한다. 해결 방법은 docker를 재시작 할수 밖에......
- jenv로 생성된 symbolic link를 Windows에서 참조하지 못한다. wsl(linux) shell에서는 디렉토리 링크에 대해서 문제 없이 사용 가능하지만, 해당 경로를 Windows 운영체제에서 접근(\\\\wsl$)시 디렉토리 링크가 파일로 참조 되는 문제가 있다.
