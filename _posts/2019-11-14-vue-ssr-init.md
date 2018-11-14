---
layout: post
title: "Vue SSR with NUXT 시작하기"
subtitle: "Vue SSR을 위해서 NUXT를 적용하여 봅시다."
categories: devlog
tags: vue
---

# Vue SSR with NUXT 
저는 요즘 VUE를 이용하여 사이드 프로젝트를 만들고 있습니다. `element-ui`, `router`, `vue-meta`, `vuex`등을 붙여서 `WEBPACK`을 기반으로 어느정도 기본 토대 모양새가 나오기 시작하였고, 이를 운영서버에 배포하여 본격적으로 운영을 해보자고 마음먹었습니다.

그런데 실제 운영에서 올리면서 `SEO`를 생각해보니 동적으로 `vue-meta`를 통해서 비동기 방식으로`뒤늦게` meta정보를 넣어줘봤자 검색 크롤로러가 검색을 통해서 제 홈페이지로 유입될 가능성이 거의 0에 수렴한다는 것을 뒤늦게 깨달았습니다.

부랴부랴 `SSR`을 적용하기로 마음먹었고, 이를 정리하여 보고자 합니다. 시작하면서 깨달은 점은 `WEBPACK`으로 개발하는 경우와 `NUXT`로 개발하는 경우 폴더구조, 설정파일 등이 상당히 다르기 때문에 거의 대부분 다시 손을 대야 하는 상황이 발생합니다. 초기에 `SSR`을 고려한다면 저 처럼 `일단 만들고 SSR 적용하자` 라고 생각하지 마시고 `처음부터 SSR로 구성하기 위해서 NUXT로 시작하자`라고 결정하는게 속 편합니다.


**Reference**
- [NUXT 공식 홈페이지(한글)](https://ko.nuxtjs.org/guide/installation){: target="_blank" }

*** 

## VUE NUXT 시작하기
공식 홈페이지에서는 init 명령어를 아래와 같이 가이드 하지만,
```
vue init nuxt-community/starter-template project-name
```
실제 nuxt-community/starter-template를 다운로드 받고 돌려보면 왜인지 es-lint관련 에러 때문에 실행이 불가능하였습니다.

대신 아래 명령어를 통해서 수행하세요.

```
vue init nuxt/starter project-name
```
nuxt/starter template의 경우는 모든게 잘 동작합니다.

종합하면
```
vue init nuxt/starter project-name
npm install
npm run dev
```
3줄의 명령어를 통해서 NUXT를 시작할 수 있습니다.

## SSR 대상 비동기 데이터 호출 ##
이제 .vue파일에 아래와 같이 `asyncData`라는 메소드를 정의할 수 있습니다.

바로 이곳에서 html 을 렌더링 하기 전에 데이터를 데이터를 송신하여 응답을 받아 화면에 매핑 할 수 있습니다.

즉, VUE 라이프 사이클이 시작되기 전에 asyncData가 먼저 호출되어 return 한 title이라는 Object를 data의 title과 병합 하여 주는 방식으로 동작합니다.

단, asyncData는 NUXT의 pages폴더 내부에 있는 .vue 파일에서만 동작합니다.

```javascript
<script>
export default {
  asyncData ({ params }) {
    return axios.get(`http://localhost:8080/product/1`)
    .then((res) => {
      return { title: res.data.title }
    })
  },
  data () {
    return {
      title: ''
    }
  }
}
</script>
```
이제 할 일은 기존 프로젝트를 NUXT 기반의 새 프로젝트로 옮기는 일이고, meta 등의 검색관련 데이터는 비동기를 통해서 미리 세팅해주는 방법을 공부해 봐야겠습니다.

**어때요? 참 쉽죠?**

![](/assets/img/postimg/bob.jpg)
