# 🌪 여정의 시작

모두가 메이저리그 야구를 즐겼으면 마음에
[MLB라는 개인 프로젝트](https://github.com/tnehd1998/MLB)를 진행하고 있었다.

초기에 기획한 기능들을 하나둘씩 차근차근 제작한 후,
프로젝트도 마무리 단계에 접어들고 있었다.

해당 프로젝트의 데이터는
단순히 axios를 통해 불러오고 있고,
Recoil을 통해 상태를 관리하고 있었다.

프로젝트를 제작한 후,
제작한 화면을 확인할 때
심기를 건드리는 무언가가 존재했다.

# 🙀 첫번째 문제

## 🖥 비효율적인 화면 전환

기능들을 구현하고 나서 화면들을 확인해보니,
특정 페이지에서 다른 페이지로 넘어간 후 다시 돌아왔을 때,
이미 받아왔던 데이터를 재활용하지 못하는 상황을 발견했다.

해당 상황을 확인한 후,
"어떻게 하면 데이터를 재활용할 수 있을까?"에
대해 고민을 시작했다.

해당 문제를 해결하면 이미 받아온 데이터를 기다릴 필요가 없어
사용자 경험도 향상될 것이 확실해 보였기 때문에
해당 상황을 개선할 해결책들을 찾아봤다.

## 💡 의외로 간단한 해결책???

데이터를 재활용하기 위해
데이터 캐싱을 중심적으로
적용할만한 기술들을 찾아봤다.

해당 프로젝트에 Recoil로 상태관리를 하고 있었고,
[Recoil의 selector를 통해 비동기 데이터 처리](https://recoiljs.org/ko/docs/guides/asynchronous-data-queries/)를 하면
데이터 캐싱을 지원해준다는 내용을 공식문서에서 확인할 수 있었다.

해당 내용을 기반으로 프로젝트에 적용을 해보니,
데이터의 재활용 문제를 해결할 수 있었다.

데이터를 재활용하기에 이미 방문한 페이지를 다시 방문할 때,
사용자들이 이미 데이터 로딩을 한 화면들을
로딩없이 다시 확인할 수 있었다.

# 😾 두번째 문제

데이터 캐싱을 통해 문제를 해결한 후,
구현한 화면들이 데이터를 재활용하며
화면 전환이 이루어지고 있었다.

생각보다 순조롭게 원하는 기능들을 구현했다는 뿌듯함을 느끼기도 전에
또 다른 문제를 발견할 수 있었다.

recoil의 selector를 통해 모든 비동기 데이터를 처리하다 보니,
상태관리 코드만 너무 비대해졌음을 확인할 수 있었다.

프로젝트를 확장함에 있어
상태관리 코드만 심각하게 비대해질 것은
확실시해보였다.

해당 상황을 통해, 비동기 데이터 처리는 api라는 폴더로 분리하고,
클라이언트 상에서 쓰이는 상태들만 상태관리를 통해 관리하는 방식으로
기능 분리를 다음 목표로 했다.

## 📌 새로운 목표

해당 상황을 통해 해결해야 하는 문제는 총 2가지였다.

1. 비대해진 상태관리에서 비동기 데이터처리 분리하기
2. 비동기 처리를 하며 데이터 캐싱하기

## 👣 제자리걸음

처음에는 둘 중 하나만 선택해야 하는 줄 알고,
recoil의 selector를 포기하고
기존 방법을 통해 데이터를 불러오기도 하고,
데이터 캐싱을 포기하고 싶지는 않아
다시 recoil의 selector를 활용하는
제자리 걸음을 반복하고 있었다.

## 👨‍👩‍👧‍👦 도움을 청해보자

당시 참여하고 있는 외부 스터디에
해당 상황에 대한 설명과 함께
해결책을 함께 찾기 위해 열띤 토론을 했었다.

하지만, 다른 스터디원들 역시 해당 상황에 대해
고민을 해봤지만 확실한 해결책을 발견하지 못해

기존의 redux를 사용하는 분은 redux-saga로,
저처럼 recoil을 사용하는 분은 selector로

비동기 데이터 처리를 하고 있다는 결론이였다.

## ✨ 한줄기의 빛

"해당 문제를 해결할 방법은 없는건가?"라는
절망에 빠지기 직전,
마지막으로 다양한 테크 블로그를 읽어보다가
우아한형제들의 테크블로그의
["Store에서 비동기 통신 분리하기"](https://techblog.woowahan.com/6339/)라는
글을 발견할 수 있었다.

해당 글은 나와 정확히 똑같은 상황에서
똑같은 고민을 하고 있었기에
순식간에 글에 몰입할 수 있었다.

해당 글을 통해 react-query라는 기술의 존재를 알았다.

## 🔥 React-query

[React-query](https://react-query.tanstack.com/)는 데이터 통신을 대신 담당해주는 라이브러리이며,
특정 키 값만 잘 지정해준다면
데이터 캐싱 역시 알아서 처리해주는
유용한 라이브러리였다.

React-query를 도입하니 해결하고 싶은 두가지 문제였던
비대해진 상태관리에서 비동기 데이터처리 분리하기와
비동기 처리를 하며 데이터 캐싱하기를 해결할 수 있었다.

코드를 보니
비동기 데이터 처리 함수는 api 폴더에,
클라이언트에서 사용되는 상태는 store 폴더로 파일을 분리하다보니,
프로젝트를 편하게 확장을 할 수 있는 형태로 리팩토링을 할 수 있었다.

# ☀️ 결론

## 😵‍💫 느낀점

해당 문제를 쉽게 해결했다면 거짓말이다.
해당 결과물을 만들기 위해
정답을 모르는 문제를
몇일동안 혼자 끙끙대며 고민을 했다.

힘들었던것은 사실이지만,
노력한 결과물을 보니,
그동안의 힘들었던 시간들을
모두 보상받는 느낌이었다.

해당 경험을 통해,
끊임없이 파고드니 해결하는 상황을 보며
"어떤 문제든 완벽한 해결책은 아니더라도
어느 정도 만족할만한 결과물을 만들어 낼 수 있구나!!"라는
것을 느낄 수 있었던 경험이였다.

## 🌈 공유하고 싶은 정보 + TMI

React-query를 접하게 해준
우아한형제들의 테크블로그의
"Store에서 비동기 통신 분리하기"라는
글은 알고보니 매우 인기있는 글이었다.

그래서 우아한형제들에서
올해 2월에 해당 글을 쓴 개발자분이
우아한테크세미나에서 해당 주제를 기반으로 발표를 진행했다.

이전부터 우아한테크세미나를 참여하고 있었기에,
해당 주제로 발표를 진행한다는 메일을 확인한 후,
신나는 마음에 참여하고 있던 스터디에
많은 사람들이 좋은 내용을 같이 들었으면하는 마음에
게시판에 함께 세미나를 듣자는 글을 쓴 기억이 난다.

해당 주제에 관심이 생겼다면 해당 발표 영상을
[2월 우아한테크세미나 영상](https://www.youtube.com/watch?v=MArE6Hy371c&t=2s)을 확인해보면 좋을것 같다.

### [테크 블로그에 그림과 함께 더 쉽게 풀어쓴 글](https://velog.io/@tnehd1998/React-Query%EB%A5%BC-MLB-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)
