---
layout: post
title: "Linux (CentOS7.5)에서 jenkins 설치"
subtitle: "Linux (CentOS7.5)에서 jenkins를 설치하여 봅시다."
categories: devlog
tags: linux database
---

# Linux (CentOS7.5)에서 jenkins을 설치하여 봅시다.
CentOS 7 Linux 환경에서 jenkins를 설치하는 과정을 설명합니다. 현재 최신버젼의 jenkins의 경우 jdk 1.7 이상을 필수로 요구하기 때문에 jdk가 설치되어 있어야 합니다.
> 참고 [Linux (CentOS7.5)에서 JDK 8을 설치하여 봅시다.](https://lee-jisoo.github.io/devlog/2018/09/18/linux-java-install/){: target="_blank" }


## 설치버젼
* CentOS Linux release 7.5.1804 (Core)
* openjdk 1.8
* jenkins-2.141-1.1

## repo 다운
`YUM` 실행을 위한 repo파일을 다운로드 합니다.
```sh
$ wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
```
실행결과
```
--2018-09-18 00:22:23--  http://pkg.jenkins-ci.org/redhat/jenkins.repo
Resolving pkg.jenkins-ci.org (pkg.jenkins-ci.org)... 52.202.51.185
Connecting to pkg.jenkins-ci.org (pkg.jenkins-ci.org)|52.202.51.185|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 71
Saving to: ‘/etc/yum.repos.d/jenkins.repo’

100%[==============================================================================>] 71          --.-K/s   in 0s

2018-09-18 00:22:25 (4.26 MB/s) - ‘/etc/yum.repos.d/jenkins.repo’ saved [71/71]
```

## rpm public key import
jenkins의 경우 아래 명령어로 rpm public key를 import 한 뒤 설치가 가능합니다.
```sh
$ rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
```

## Jenkins 설치
```
$ yum install jenkins
```
실행결과
```
...(생략)
========================================================================================================================
 Package                     Arch                       Version                       Repository                   Size
========================================================================================================================
Installing:
 jenkins                     noarch                     2.141-1.1                     jenkins                      72 M

Transaction Summary
========================================================================================================================
Install  1 Package

Total size: 72 M
Installed size: 72 M
Is this ok [y/d/N]: ㅛy
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : jenkins-2.141-1.1.noarch                                                                             1/1
  Verifying  : jenkins-2.141-1.1.noarch                                                                             1/1

Installed:
  jenkins.noarch 0:2.141-1.1
```

## Jenkins port 및 기본설정 변경
Jenkins 기본설정은 `/etc/sysconfig/jenkins` 파일에서 관리합니다.
```
$ vi /etc/sysconfig/jenkins
```
필요시 적절히 설정을 변경하고 저장합니다.
> JENKINS_PORT="9090"

변경된 포트의 방화벽을 오픈합니다.
```
$ firewall-cmd --permanent --add-port=9090/tcp
```

## Jenkins Service 시작/종료
**시작**
```
$ /etc/init.d/jenkins start
```
**종료**
```
$ /etc/init.d/jenkins stop
```

## Jenkins 초기접속
http://serverip:9090 로 접속하여 초기접속을 진행합니다.
![](/assets/img/postimg/2018-09/2018-09-18-01.png)

초기 접속시 서버에 생성된 기본 패스워드로 진입을 하면 됩니다.
```
$ cat /var/lib/jenkins/secrets/initialAdminPassword
yourpassword
```
