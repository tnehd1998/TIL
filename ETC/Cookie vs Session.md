# 쿠키와 세션을 사용하는 이유

HTTP 프로토콜은
Connectionless이며 Stateless라는 특징을 지닌
프로토콜의 한 종류이다.

## Connectionless Protocol

클라이언트가 서버에게 특정 요청을 했을 때,
해당 요청에 대한 응답 이후에 연결을 끊는 방식을 의미한다.

## Stateless Protocol

클라이언트의 현재 지니고 있는 상태 정보를
가지지 않는 서버 처리 방식이다.

클라이언트와의 통신을 통해 데이터를 주고 받았다 해도,
바로 다음 통신에서 방금 전달한 데이터를
유지하지 않는 것을 의미한다.

## HTTP의 특징으로 인한 문제점

Stateless라는 특징으로 인해
사용자는 다른 페이지로 넘어갈 때마다
로그인을 다시하거나,

특정 상품을 장바구니에 추가했는데
선택한 상품이 존재하지 않는
상황들이 발생할 수 있다.

즉, 클라이언트 상태를 필요로 하는
Stateful한 특징을 띄는 서비스가 점점 많아졌다.

다음과 같이 Stateless로 인해
발생하는 불편한 사용자 경험을 해결하기 위해
생각해낸 해결책이 쿠키와 세션의 사용이다.

즉, 클라이언트와 주고 받은 정보를 유지하는
Stateful한 성격을 띄게하기 위해
사용되는 것이 쿠키와 세션이다.

# Cookie

Cookie란 사용자가 특정 웹 사이트에 방문할 경우,
웹 서버가 사용자의 컴퓨터에
파일 또는 메모리에 저장하는 작은 기록 정보 파일이다.

파일에 담긴 정보는 인터넷 사용자가
같은 웹 사이트에 다시 방문했을 때,
필요시 참고할 수 있는 정보를 의미한다.

## Cookie의 구성 요소

쿠키는 다음과 같은 구성 요소를 지니고 있다.

1. 이름 (Name) : 쿠키를 구별하는데 사용
2. 값 (Value) : 각 쿠키에 저장된 값을 의미
3. 유효기간 (Expires) : 쿠키가 유지되는 기간을 의미
4. 도메인 (Domain) : 쿠키가 사용되는 도메인을 의미
5. 경로 (Path) : 쿠키를 전송할 경로를 의미

## Cookie의 종류

쿠키의 종류는 크게 4가지가 존재한다.

1. Session Cookie : 만료시간을 설정하고 메모리에만 저장되며 브라우저 종료시 해당 쿠키는 삭제된다.
2. Persistent Cookie : 장기간 유지되는 쿠키이며, 파일로 저장되어 브라우저 종료와 관계없이 사용할 수 있다.
3. Secure Cookie : HTTPS에서만 사용되며, 쿠키 정보가 암호화되어 전송된다.
4. Third-Party Cookie : 유입 경로를 추적하기 위해 사용되며, 광고 배너 등을 관리할 때 사용된다.

## Cookie의 동작 원리

1. 클라이언트가 페이지를 요청한다.
2. 서버는 쿠키를 생성한다.
3. HTTP 헤더에 생성한 쿠키를 포함시켜 응답한다.
4. 넘겨 받은 쿠키 만료 기간이 될때까지 클라이언트에서 보관한다.
5. 같은 요청을 하는 경우 HTTP 헤더에 쿠키를 함께 전송한다.
6. 서버는 전달받은 쿠키를 통해 변경 할 필요가 있을 때 쿠키를 업데이트해서 변경된 쿠키를 HTTP 헤더에 포함시켜 응답한다.

해당 방식으로 작동하는 Cookie를 통해,
아이디와 비밀번호를 기억한 후,
자동입력 해주는 자동로그인과

특정 웹사이트의 팝업창의
“오늘 이 창을 다시 보지 않기"와 같은
기능을 가능하게 해준다.

## Cookie의 문제점

쿠키에 대한 정보를 Header에 포함시켜
데이터를 전송하기 때문에 상당한 트래픽을 요구하게 된다.

또한 결제정보와 같은 유출되면 안되는 정보들을 쿠키를 통해 저장하면
보안에 대한 심각한 문제점도 발생하게 된다.

다음과 같이 쿠키의 트래픽 문제와 보안적인 이슈를 해결하기 위해
생각해낸 방법이 세션이다.

# Session

세션 역시 쿠키형태로 데이터를 식별하지만
브라우저에 저장하지 않고 서버 측에서 관리를 한다.

브라우저가 종료될 때까지 인증상태를 유지하지만,
메모리에 저장을 하기 때문에 브라우저가 종료되면
인증 정보가 사라진다는 특징을 지니고 있다.

## Session의 동작 원리

1. 클라이언트가 서버에 특정 데이터를 요청한다.
2. 서버는 HTTP 요청을 통해 쿠키에서 Session id를 확인한 후, 쿠키가 존재하지 않으면 Set-Cookie를 통해 새로 발행한 Session-id를 전송한다.
3. 클라이언트는 HTTP 헤더에 전달 받은 Session id 포함하여 데이터 요청을 진행한다.
4. 서버는 Session id를 통해 클라이언트 상태 정보를 유지하며 적절한 응답을 진행한다.

해당 방법을 통해
화면이 이동해도 로그인이 풀리지 않고
로그아웃하기 전 또는 브라우저를 종료할 때까지
로그인이 유지되는 기능을 가능하게 한다.

서버에 쿠키를 저장하기에
관리하기가 매우 편하고
보안 면에서도 쿠키보다 훨씬 우수하다.

## Session의 문제점

서버에 쿠키를 저장하기에
사용자가 많아질수록 서버 메모리를 많이 차지하기에
속도와 비용의 문제가 발생할 수 있다.

또한 세션 정보를 하나의 저장장치에
공유하며 사용하기 때문에
load-balancing 문제가 발생한다.

# 결론

정확하게 설명을 하자면,
쿠키와 세션 모두 쿠키를 기반으로 작동하지만
저장되는 위치가 브라우저인지 서버인지의 차이가 있다.

보안 측면에서는 세션이 서버에 저장되기에 쿠키보다 우수하지만,
속도 측면에서는 쿠키가 서버의 처리를 필요로 하는 세션보다 훨씬 빠르다.

이처럼 장단점이 서로 다르기 때문에
어떤 서비스를 사용하고 런칭하냐에 따라
유동적으로 쿠키와 세션을 선택하는 것이 올바른 선택이다.

# 참고한 자료

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Web/Cookie %26 Session.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Web/Cookie%20%26%20Session.md)
- [https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/Cookie_Session.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/Cookie_Session.md)
- [https://github.com/dongkyun-dev/TIL/blob/master/web/cookie_session.md](https://github.com/dongkyun-dev/TIL/blob/master/web/cookie_session.md)
- [https://interconnection.tistory.com/74](https://interconnection.tistory.com/74)
- [https://nesoy.github.io/articles/2017-03/Session-Cookie](https://nesoy.github.io/articles/2017-03/Session-Cookie)
- [https://hahahoho5915.tistory.com/32](https://hahahoho5915.tistory.com/32)
