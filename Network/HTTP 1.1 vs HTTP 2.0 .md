# HTTP 1.1란?

HTTP 1.1의 등장으로
GET, POST만 존재했던 HTTP 1.0에서
PUT, DELETE가 추가되었다.

# HTTP 1.1의 특징

## HTTP Pipelining

HTTP Pipelining은 클라이언트와 서버간 매번 발생하는
요청과 응답의 효율성을 개선하기 위해
만들어진 기능이다.

HTTP 1.1 이전의 연결 방식은
특정 요청과 응답마다
연결과 종료를 계속해서 진행된다.

해당 상황으로 인해
TCP의 handshake가 매번 발생하게 되고,
해당 상황은 Network Latency를 발생시킨다.

파이프라이닝은 모든 요청에 대한 응답을 기다리지 않고,
여러개의 HTTP 요청을 이미 연결된 하나의 TCP를 통해
계속해서 다음 요청을 하는 원리로 작동된다.

파이프라이닝으로 인해 하나의 연결로
다수의 요청과 응답을 처리할 수 있기에
Network Latency를 줄여주게 된다.

## Persistent Connection

HTTP 1.1의 연결은
TCP의 3 way handshake 방식으로
연결이 이루어진다.

Persistent Connection은
해당 3 way handshake를 통한 TCP 연결을
재활용하는 원리를 의미한다.

이미 연결된 Connection을 재활용하며
TCP handshake로 발생하는 비용을
많이 아낄수 있게 된다.

만약 연결을 해제하고 싶다면,
Connection: close라는 명시적 헤더를 이용하여
특정 연결에 대한 해제를 진행할 수 있다.

# HTTP 1.1의 문제점

HTTP 1.1은 기본적으로
연결 당 하나의 요청과 응답을 처리할 수 있다.

그렇기에 동시전송 문제와 같은
다수의 리소스를 처리하기에
속도와 성능 이슈가 발생하게 된다.

## HOL (Head Of Line) Blocking

HTTP 1.1은 연결 당 하나의 요청을
개선하기 위해 Pipelining을 활용하게 된다.

이를 통해 하나의 연결로
다수의 파일을 요청 및 응답을 받을 수 있게 되었다.

하지만 특정 이미지 3종류가 있다고 가정해보자

- 첫번째 : 10초 소요
- 두번째 : 0.2초 소요
- 세번째 : 0.1초 소요

순서대로 첫번째 이미지에 대한 요청 및 응답 후,
다음 이미지를 요청하게 된다.

위와 같은 상황에서 두번째와 세번째는
처리 속도가 오래 걸리는 첫번째 이미지의 처리가
끝날때까지 대기해야 한다.

이와 같이
먼저 요청된 오래 걸리는 작업을
끝날때까지 대기하는 상황을 HOL라고 부르며,
Pipelining의 문제점 중 하나이다.

## RTT(Round Trip Time) 증가

TCP를 활용하여 특정 요청을 처리하기 위해
3 way handshake 과정을 매번 거쳐야한다.

즉, 매 요청 및 응답마다
연결 및 종료가 일어난다는 의미기도 하다.

이러한 불필요한 RTT의 증가로 인해
네트워크 지연이 발생해
성능을 저하시키는 문제가 발생한다.

예전에는 컨텐츠가 지금처럼 많지 않았기에
3 way handshake를 통한 TCP 연결이
전혀 부담되지 않았다.

하지만 컨텐츠가 점점 증가하면서
너무 많은 3 way handshake를 통한 TCP 연결이
매우 부담스러운 작업이 되어버렸다.

## 무거운 Header 구조

HTTP 1.1는
헤더속 많은 메타 정보들이
저장되어 통신이 이뤄지게 된다.

매 요청마다 중복되는 헤더 값을 전송하게 되며
cookie 정보도 매 요청마다 헤더에 포함되어 전송하게 된다.

이와 같이 헤더에 많은 요소들이 들어있기에
요청한 데이터보다 헤더 값이 더 커지는
비효율적인 상황이 종종 발생한다.

# HTTP 1.1를 살려내기 위한 노력들

HTTP 1.1로 인해 발생하는
치명적인 단점들을 해결하기위해
조금이라도 더 빠르고 효율적으로 데이터를 제공하는
몇가지 방법들이 존재한다.

### Image Spriting

이미지 스프라이팅은 사용되는 모든 아이콘을
하나의 큰 이미지로 제작한 다음
한번에 모든 아이콘을 한 이미지 파일로 넘겨준다.

해당 방법을 통해,
모든 아이콘을 나눠서 요청 및 응답을 받는 것이 아닌
한번의 요청으로 모든 아이콘을 받아오기에
요청 횟수를 최소한으로 할 수 있게 된다.

### Domain Sharding

도메인 분할 기법은 여러 도메인을 활용하여
다수의 연결을 생성해 병렬적으로 요청을 보내는 방법이다.

해당 방법을 통해 전에 제기되었던 문제인
HOL Blocking과 같은 문제가 어느정도 해결된다.

또한 특정 도메인마다 요청하는 데이터를 분리하여
헤더 및 쿠키의 크기를 줄일 수 있다는 장점도 존재한다.

하지만, 도메인당 연결 개수의 제한이 존재하고
몇개의 도메인을 사용할 것인지 판단하는 것은 쉽지 않기에
이 또한 근본적인 해결책으로 보기는 힘들다.

### Minify CSS/JavaScript

요청을 통해 전송되는 데이터의 용량을 줄이기 위해
CSS/JavaScript의 크기롤 축소하여 전달하는 방법이다.

해당 방식은 요청하는 파일들의 크기를 최소한함으로써
조금이라도 더 빠른 요청 처리를 위해 사용하는 기법이다.

### Data URI Scheme

HTML 문서 내 이미지 요소를 base64로 인코딩된
이미지 데이터를 직접 기술하는 방식이다.

해당 방식은 요청 수를 조금이라도
더 줄이기 위해 사용하는 기법이다.

### Load Faster

css요소인 스타일시트는
HTML 문서의 상단에 배치하고,
js요소는 스크립트는
HTML 문서의 하단에 배치하는 것이다.

# SPDY 프로토콜

HTTP 1.1을 살려내기 위한 노력에도 불구하고,
HTTP 1.1이 가지고 있던 모든 단점들을 해결할 수는 없었다.

구글은 HTTP 1.1의 문제 중
Network Latency에 집중을 하여
더 빠르고 효율적인 HTTP인
SPDY라는 새로운 프로토콜을 생성했다.

SPDY는 HTTP를 대체하는 프로토콜이 아닌,
HTTP가 전송 계층을 통해 전송되는 방식을
재정의하는 프로토콜이다.

그렇기에 전송 계층의 구현만 변경하면
기존 HTTP 서버 프로그램을
그대로 SPDY에서 사용할 수 있게 된다.

현재는 SPDY가 쓰이지는 않지만
HTTP 1.1에 비해 상당한 성능 향상과 효율성을 보여줬으며,
HTTP 2.0의 초안에서 SPDY 규격이 참고되었다.

# HTTP 2.0란?

HTTP 2.0은 SPDY를 기반으로
처음 구현된 새로운 프로토콜이다.

HTTP 2.0은 사실 새로운 프로토콜을
만든것이 아닌 성능 향상에 초점을 둔
향상된 HTTP 1.1 프로토콜이다.

# HTTP 2.0의 특징

## Multiplexed Streams

멀티플렉싱은 한 커넥션을 통해
여러 개의 스트림을 동시에 열 수 있게 한다.

하나의 HTTP 2.0 커넥션을 통해
여러 개의 요청이 동시에 보내질 수 있으며,
HTTP 1.1의 파이프라이닝과 지속적 연결이
개선된 방법이라 할 수 있다.

## Stream Prioritization

HTTP 2.0은 HTTP 1.1와 달리
리소스간의 우선순위를 설정한다.

더 빨리 처리할 수 있는 리소스의
우선순위를 상위로 설정하여
HTTP 1.1의 HOL Blocking과 같은
문제를 해결한다.

## Header Compression

HTTP 1.1의 경우 두개의 요청의 헤더에 중복 값이 존재해도
해당 값들을 중복하여 전송한다.

하지만, HTTP 2.0은 헤더에 중복되는 값이 존재하는 경우
정적/동적 헤더 테이블을 활용하여
중복 헤더를 검출하여 중복된 헤더는 인덱스 값만 전송하고
새로운 헤더 정보는 허프만 인코딩 기법으로 인코딩하여 처리한다.

해당 허프만 인코딩 기법을 HPACK 압축방식이라 부르며
별도의 명세서(RFC 7531)로 관리한다.

## Server Push

특정 HTML 문서를 클라이언트에서 요청하면,
HTML 문서에 포함되어 있는 리소스들(CSS, JS)을
추가로 요청할 것이라는 예측을 할 수 있다.

그렇기에 클라이언트에서 HTML 문서에 포함되어 있는
리소스들(CSS, JS)에 대한 추가 요청을 보내지 않아도
서버에서 알아서 HTML 문서에 필요한 리소스들을
전송해주는 원리를 의미한다.

HTTP 2.0은 클라이언트가 요청하기 전에
리소스들을 Push해주는 방법으로
클라이언트의 요청을 최소화하여 성능을 향상시킨다.

# 참고한 자료

- [https://ijbgo.tistory.com/26](https://ijbgo.tistory.com/26)
- [https://github.com/dongkyun-dev/TIL/blob/master/web/HTTP1.1과 HTTP2.0%2C 그리고 간단한 HTTP3.0.md](https://github.com/dongkyun-dev/TIL/blob/master/web/HTTP1.1%EA%B3%BC%20HTTP2.0%2C%20%EA%B7%B8%EB%A6%AC%EA%B3%A0%20%EA%B0%84%EB%8B%A8%ED%95%9C%20HTTP3.0.md)
- [https://jins-dev.tistory.com/entry/HTTP11-의-HTTP-Pipelining-과-Persistent-Connection-에-대하여](https://jins-dev.tistory.com/entry/HTTP11-%EC%9D%98-HTTP-Pipelining-%EA%B3%BC-Persistent-Connection-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)
- [https://www.popit.kr/나만-모르고-있던-http2/ 출처: https://ijbgo.tistory.com/26 [한량 개발자:티스토리]](https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-http2/%20%EC%B6%9C%EC%B2%98:%20https://ijbgo.tistory.com/26%20%5B%ED%95%9C%EB%9F%89%20%EA%B0%9C%EB%B0%9C%EC%9E%90:%ED%8B%B0%EC%8A%A4%ED%86%A0%EB%A6%AC%5D)
- [https://ko.wikipedia.org/wiki/SPDY](https://ko.wikipedia.org/wiki/SPDY)
