---
layout: post
title: "Jenkins 빌드 실패시 라인 알림톡 보내기"
subtitle: "Jenkins 빌드 실패시 라인으로 알람톡을 보내 봅시다."
categories: devlog
tags: linux java
header-img: "/img/postimg/line-thumb.png"
---

# Jenkins 빌드 실패시 라인으로 알람톡을 보내 봅시다.
Jenkins를 이용하여 CI환경으로 사용하거나 배치 스케쥴러 환경으로 사용하는 경우가 많이 있습니다. 저 역시 신규로 구축하는 서비스를 Jenkins를 이용하여 `빌드배포` 및 `배치스케쥴러` 로 이용하고 있습니다. 그런데 배치스케쥴러가 실패할 경우 개인이 이를 매일 같이 Jenkins에 접속하여 확인하는 것은 여간 번거로운 일이 아닙니다.

그래서 보통의 경우 `이메일을 통한 Notify를 설정`하곤 합니다. 하지만 이메일은 아무래도 메신저 보다는 접근성이 떨어지기 때문에 메신저를 이용한 Notify 환경을 구축해보고 싶었습니다. 고려대상은 `카카오톡 알림톡`과 `라인 Notify` 서비스 였습니다. 

결론적으로 `카카오톡 알림톡`을 하고 싶었으나 `유료`이기 때문에 `무료`서비스인 `라인 Notify`를 통해서 배치 실패시 라인을 통하여 메시지를 보내도록 설정하였습니다. 생각보다 엄청 간단하게 설정이 가능하기 때문에 Jenkins를 빌드 실패시 알림을 설정하고자 하시는 분들을 참고하시면 좋겠습니다.


**Reference**
- [라인 Notify 서비스](https://engineering.linecorp.com/ko/blog/detail/88){: target="_blank" }
- [카카오톡 알림톡 API](https://www.apistore.co.kr/api/apiProviderGuide.do?service_seq=558){: target="_blank" }

*** 

## 라인 알람톡 Personal access token 발행
첫 번째는 라인 알람톡을 통해서 P`ersonal access token`을 발행 받아야 합니다.

https://notify-bot.line.me/my/ 로 접속한 뒤 `Generate token`을 선택합니다.
![](/assets/img/postimg/2018-09/2018-09-22-line.png)

- Token은 여러번 발급이 가능하며 각 Token마다 알람을 보낼 대화방을 지정할 수 있습니다.
- Token Name : 토큰을 구분하기 위한 이름으로 아무거나 입력하시면 됩니다.
- Notifications Group : 이미 열러져 있는 대화방 목록이 나열됩니다. 선택한 대화방에 알람이 가게 됩니다. 

저는 가장 위에 있는 저 혼자만 메시지를 받을 것이기 때문에 `1-on-1 chat with LINE Notify`를 선택하였습니다.

# curl 을 통한 메시지 보내기
Token 생성이 완료되었다면 발급받은 키를 이용하여 아래 URL 을 호출하여 봅시다.

```sh
$ curl -X POST -H 'Authorization: Bearer token_key' -F message="안녕하세요 ㅎㅎㅎ" https://notify-api.line.me/api/notify
```
token_key로 되어 있는 부분은 자신이 발급받은 Token값을 입력하여 준 뒤 명령어를 실행하여 봅시다.
![](/assets/img/postimg/2018-09/2018-09-22-line2.jpg)


