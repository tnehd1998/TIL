# this란?

자바스크립트에서 this 키워드는
다른 언어와는 완전히 다른 개념이다.

Java와 같은 다른 언어에서 this는
자기 자신을 가리킬 수 있는
참조 변수로 사용이 된다.

하지만, 자바스크립트의 this는
함수 호출 방식에 따라
this에 바인딩되는 객체가
동적으로 결정된다.

# 함수 호출 방식

this 바인딩은 함수 호출 방식에 따라
동적으로 결정되며
함수를 호출하는 방식은 크게
4가지로 분류해 볼 수 있다.

- 바인딩 : 식별자와 값을 연결하는 과정

## 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩된다.

```jsx
function f1() {
  return this;
}

// 브라우저
f1() === window; // true

// Node.js
f1() === global; // true
```

어떠한 함수라도 일반 함수로 호출되면
this에 전역 객체가 바인딩된다.

## 메서드 호출

메서드의 경우
특정 메서드를 호출할 때 메서드 이름 앞의
마침표 연산자 앞에 기술한 객체가 바인딩된다.

```jsx
const billy = {
  name: "Lee",
  sayName: function () {
    console.log(this.name);
  },
};

const elon = {
  name: "Musk",
};

elon.sayName = billy.sayName;

billy.sayName(); // "Lee"
elon.sayName(); // "Musk"
```

이와 같이 메서드 내부의 this는
메서드를 소유한 객체가 아닌
메서드를 호출한 객체에 바인딩된다.

## 생성자 함수 호출

생성자 함수 내부의 this에는
생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

new 연산자와 함께 생성자 함수를 호출하면
일반 함수나 메서드와는 다르게 동작한다.

만약 new 연산자와 함께 생성자 함수를 호출하지 않으면
생성자 함수가 아닌 일반 함수로 동작한다.

다음과 같은 혼란을 방지하기 위해
생성자 함수명의 첫문자는 대문자로 정의한다.

## Function.prototype.apply / call / bind

apply, call, bind 메서드는 모두
Function.prototype의 메서드다.

그렇기에 모든 함수가
해당 메서드들을 상속받아 사용할 수 있다.

다음과 같은 메서드들은
this의 역할을 직접 명확하게 지정해주는 방법이다.

### apply, call

apply와 call 메서드는
함수를 호출할 때 사용된다

apply와 call 메서드는
호출할 함수에 인수를 전달하는 방식만 다를 뿐
동일하게 동작한다.

- apply : 호출할 함수의 인수를 배열로 묶어 전달한다
- call : 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다

```jsx
function getThisBinding() {
  console.log(arguments);
  return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}

console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}
```

### bind

bind 메서드는 메서드의 this와
메서드 내부의 중첩함수 또는 콜백 함수의 this가
불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```jsx
function getThisBinding() {
  return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.bind(thisArg));
// function getThisBinding(){
//    return this;
// }
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

bind 메서드를 사용하면
특정 함수의 this값을 영구적으로 바꾼다.

그렇기에, bind 메서드를 사용하려면
this가 어디에 바인딩되는지
정확히 파악하며 사용해야 한다.

# 참고한 자료

- 모던 자바스크립트 Deep Dive
- [https://poiemaweb.com/js-this](https://poiemaweb.com/js-this)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)
- [https://im-developer.tistory.com/96](https://im-developer.tistory.com/96)
- [https://github.com/dongkyun-dev/TIL/blob/master/JS/👏Call%2C Apply%2C Bind.md](https://github.com/dongkyun-dev/TIL/blob/master/JS/%F0%9F%91%8FCall%2C%20Apply%2C%20Bind.md)
