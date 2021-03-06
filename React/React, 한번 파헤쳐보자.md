# React, 한번 파헤쳐보자!

#### React의 작동 원리와 배경 지식으로 인해 조금은 React와 친숙해졌다고 느껴지시나요? 이번에는 React의 사용 방법에 대해서 알아보겠습니다.

## React 작동 원리

#### React는 webpack이라는 번들러를 통해 react 모듈을 모두 합쳐서 하나의 파일로 불러와 사용합니다.

#### ES6 이전 버전으로 코드를 다운그레이드하고 싶다면 babel을 사용하여 ES6 이전 문법으로 변환도 가능합니다.

#### React는 js파일을 쓰지 않고, html을 닮은 jsx라는 파일을 구성하여 프로그램을 작동시킵니다.

## JSX

#### React에서 컴포넌트를 만들 때 사용하는 파일이며 컴포넌트 단위로 파일을 나누는 React에 최적화 된 형식이라고 볼 수 있습니다.

### JSX를 주로 쓰게되는 이유

> 1. 사진만 봐도 알수 있듯이 JSX 코드는 매우 직관적이며 가독성이 높습니다.
> 2. 재활용하기가 매우 편해서, DRY한 코드를 추구하는 React의 컨셉과 매우 잘맞습니다.

### JSX 사용규칙

#### 모든 파일에 사용 방법과 그들만이 규칙이 있듯이, JSX 역시 파일을 사용하기 위해선 지켜야 될 몇가지 규칙들이 존재합니다.

> 1. 반드시 부모 요소 하나로 감싸도록 파일을 구성한다.
> 2. 자바스크립트 표현을 JSX에서 사용하고 싶다면 {}를 감싸서 사용한다.
> 3. if문을 쓰기보단 삼항 연산자, &&를 활용한다.

#### 이외의 많은 규칙들이 존재하며 더 알아보고 싶다면 공식 문서를 참고하면 좋을것 같습니다 😎

[리액트 공식 문서 - JSX](https://ko.reactjs.org/docs/jsx-in-depth.html)

## 컴포넌트

#### React는 기본적으로 컨셉은 코드를 DRY하게 유지하는 것입니다. 그렇기 때문에 컴포넌트 별로 기능 및 화면을 나눠 재활용성을 극대화합니다.

### State

- 컴포넌트 내부에서 바뀔수 있는 값, 즉 상태를 의미합니다.
- state값을 바꿀때는 무조건 세터함수인 setState를 통해 바꿀수 있다.
- 기존 state는 게터 함수라고 이해하시면 편할 것 같습니다.

### Props

- properties를 줄인 약자로, 부모 컴포넌트의 state 값을 전달받아 자식 컴포넌트에서 사용할 때 사용합니다.
- 보통 부모 컴포넌트에 state의 값을 전달하면 자식 컴포넌트에서 props로 받아 해당 값을 사용할 수 있습니다.

## 라이프사이클

- 라이브사이클은 컴포넌트의 생명 주기를 나타내는 과정 및 메서드입니다.
- 말 그대로 컴포넌트가 생기고, 바뀌고, 없어지는 시기에 특정 작동을 하도록 사용되는 유용한 메서드입니다.

### 라이프사이클 메서드 종류

#### 라이프 사이클은 총 9가지가 있으며 해당 메서드에 따라 실행되는 순서가 달라집니다.

- 해당 접두사를 붙히면

  - Will : 작동하기 전
  - Did : 작동한 후

- 라이프 사이클의 종류
  - Mount : 웹 브라우저에 나타날 때
  - Update : 무언가 바뀌었을 때
  - Unmount : 웹 브라우저에서 사라질 때

### 컴포넌트의 종류

- 클래스형, 함수형 컴포넌트 두가지 종류가 존재합니다.

- 클래스형 컴포넌트는 상태를 나타내는 state, 라이프사이클 기능, 그리고 임의 메서드를 정의할 때 사용되는 컴포넌트입니다.

- 함수형 컴포넌트는 render함수가 꼭 필요하고, 그 안에서 보여줘야하는 JSX를 반환하는 용도로만 사용합니다. 즉 특정 화면을 단지 보여주는 용도로 함수형 컴포넌트를 사용했었습니다.

- 클래스형과 달리 함수형 컴포넌트의 너무 많은 장점들이 존재했습니다.

  > 1. 선언하기가 훨씬 편하다
  > 2. 메모리 자원도 클래스 컴포넌트보다 덜 사용한다.
  > 3. 배포를 할 때도 파일 크기가 더 작다

- 확실한 장점을 가지고 있음에도 불구하고 state, 라이브사이클을 사용할 수 없다는 점 때문에 클래스형 컴포넌트로 코드를 짤수 밖에 없었습니다.

- 하지만 2018년 페이스북에 의해 React의 모든 생태계가 뒤집혔습니다. 바로 React Hooks을 발표했기 때문입니다. 해당 내용은 너무 많기 때문에 다음 글에서 자세하게 설명해보겠습니다.

## 3줄 요약

> - React는 JSX 파일을 사용한다.

- 라이프사이클은 원하는 시기에 원하는 작동을 할 수 있게 한다.
- 컴포넌트는 클래스형과 함수형 두가지 종류가 존재한다.

### 마치며

- 오늘은 React를 구성하는 파일 JSX, 라이프사이클 메서드, React를 이루고 있는 컴포넌트에 대한 내용이였습니다. 해당 내용을 적어보면서 구글링을 하며 다른 블로그들을 확인하는데 역시 공식 문서에 정확한 내용과 예시가 충분히 존재한다는 것을 다시 한번 깨달을 수 있었습니다.❤️

### 참고한 자료

- https://www.zerocho.com/category/React/post/579b5ec26958781500ed9955
- https://bgeun2.tistory.com/19
- https://ko.reactjs.org/docs/state-and-lifecycle.html

### [테크 블로그에 그림과 함께 더 쉽게 풀어쓴 글](https://velog.io/@tnehd1998/React-%ED%95%9C%EB%B2%88-%ED%8C%8C%ED%97%A4%EC%B3%90%EB%B3%B4%EC%9E%90)
