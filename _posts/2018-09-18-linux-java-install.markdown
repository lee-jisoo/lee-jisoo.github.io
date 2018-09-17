---
layout: post
title: "Linux (CentOS7.5)에서 JDK 8 설치"
subtitle: "Linux (CentOS7.5)에서 JDK 8을 설치하여 봅시다."
categories: devlog
tags: linux database
---

# Linux (CentOS7.5)에서 JDK 8을 설치하여 봅시다.
CentOS 7 Linux 환경에서 Open JDK 8을 설치하는 과정을 설명합니다.

## 설치버젼
* CentOS Linux release 7.5.1804 (Core)
* openjdk version "1.8.0_181"

## 설치가능확인
**yum으로 설치 가능한 jdk 목록 확인**
```sh
$ yum list java*jdk-devel
```
**실행결과**
```sh
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: ftp.iij.ad.jp
 * epel: mirror.dmmlabs.jp
 * epel-debuginfo: mirror.dmmlabs.jp
 * epel-source: mirror.dmmlabs.jp
 * extras: ftp.iij.ad.jp
 * updates: ftp.iij.ad.jp
Available Packages
java-1.6.0-openjdk-devel.x86_64        1:1.6.0.41-1.13.13.1.el7_3        base
java-1.7.0-openjdk-devel.x86_64        1:1.7.0.191-2.6.15.4.el7_5        updates
java-1.8.0-openjdk-devel.i686          1:1.8.0.181-3.b13.el7_5           updates
java-1.8.0-openjdk-devel.x86_64        1:1.8.0.181-3.b13.el7_5           updates
```
## 설치하기
**OPEN JDK 1.8.0 설치**
```sh
$ yum install java-1.8.0-openjdk-devel.x86_64
```
**실행결과**
```sh
...(생략)
Installed:
  java-1.8.0-openjdk-devel.x86_64 1:1.8.0.181-3.b13.el7_5

Dependency Installed:
  copy-jdk-configs.noarch 0:3.3-10.el7_5                 fontconfig.x86_64 0:2.10.95-11.el7
  fontpackages-filesystem.noarch 0:1.44-8.el7            giflib.x86_64 0:4.1.6-9.el7
  java-1.8.0-openjdk.x86_64 1:1.8.0.181-3.b13.el7_5      java-1.8.0-openjdk-headless.x86_64 1:1.8.0.181-3.b13.el7_5
  javapackages-tools.noarch 0:3.4.1-11.el7               libICE.x86_64 0:1.0.9-9.el7
  libSM.x86_64 0:1.2.2-2.el7                             libXcomposite.x86_64 0:0.4.4-4.1.el7
  libXext.x86_64 0:1.3.3-3.el7                           libXfont.x86_64 0:1.5.2-1.el7
  libXi.x86_64 0:1.7.9-1.el7                             libXrender.x86_64 0:0.9.10-1.el7
  libXtst.x86_64 0:1.2.3-1.el7                           libfontenc.x86_64 0:1.1.3-3.el7
  libxslt.x86_64 0:1.1.28-5.el7                          lksctp-tools.x86_64 0:1.0.17-2.el7
  lyx-fonts.noarch 0:2.2.3-1.el7                         python-javapackages.noarch 0:3.4.1-11.el7
  python-lxml.x86_64 0:3.2.1-4.el7                       ttmkfdir.x86_64 0:3.0.9-42.el7
  tzdata-java.noarch 0:2018e-3.el7                       xorg-x11-font-utils.x86_64 1:7.5-20.el7
  xorg-x11-fonts-Type1.noarch 0:7.5-9.el7

Dependency Updated:
  nspr.x86_64 0:4.19.0-1.el7_5                nss.x86_64 0:3.36.0-5.el7_5          nss-softokn.x86_64 0:3.36.0-5.el7_5
  nss-softokn-freebl.x86_64 0:3.36.0-5.el7_5  nss-sysinit.x86_64 0:3.36.0-5.el7_5  nss-tools.x86_64 0:3.36.0-5.el7_5
  nss-util.x86_64 0:3.36.0-1.el7_5
```

## 설치확인
**JAVA버젼확인**
```sh
$ java -version
```
**실행결과**
```sh
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
```

## JAVA_HOME 환경변수 등록
**javac 위치확인**

`readlink -f`는 심볼릭 링크로 이루어진 javac명령어의 물리위치를 찾아내는 방법입니다.
```sh
$ which javac
/usr/bin/javac
$ readlink -f /usr/bin/javac
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64/bin/javac
```
**JAVA_HOME 등록**

환경변수 등록을 위하여 /etc/profile 파일을 편집합니다.
```sh
vi /etc/profile
```
마지막에 아래 내용을 추가 합니다.
> export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64/bin/javac

**환경변수 확인**
```sh
$ source /etc/profile
$ echo $JAVA_HOME
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64/bin/javac
```
`source`명령을 통해서 profile을 재로딩 한 뒤 JAVA_HOME을 확인합니다.







