# HTTP란?

HTTP는 HyperText Transfer Protocol를 의미하며, 말 그대로 클라이언트와 서버가 지정한 모델에 따라 데이터를 주고 받기 위해 사용되는 프로토콜입니다.

HTTP는 OSI 7계층 중 마지막인 어플리케이션 계층에 속하는 프로토콜 중 하나입니다.

- HTTP는 인터넷을 통해 HyperText를 주고 받기 위한 통신 규약이며 80번 포트를 사용하고 있습니다.
- 그러므로 HTTP 서버는 80번 포트에서 데이터 요청을 기다리고 있고, 클라이언트는 원하는 데이터의 요청을 80번 포트로 보내게 됩니다.

# HTTP의 문제점

HTTP 통신은 클라이언트와 서버간의 통신을 할 때, 별 다른 보안 조치가 존재하지 않기에 제3자가 전송되는 데이터를 모두 조회할 수 있다는 문제점이 존재합니다.

- 만약 비밀번호나 주민등록번호와 같은 개인 정보를 주고 받는 통신을 한다면 개인정보 유출과 같은 문제가 발생할 가능성이 매우 높습니다.
- 해당 문제를 해결하기 위해 탄생한 것이 바로 HTTPS입니다.

# HTTPS란?

HTTPS는 HyperText Transfer Protocol Secure를 의미하며, 어플리케이션 계층에 새로 만들어진 프로토콜이 아닌 HTTP에 데이터 암호화(Secure)가 추가된 프로토콜입니다.

- HTTPS는 기존 80번 포트를 사용하던 HTTP와 다르게 443번 포트를 사용하여, 제3자가 통신을 통해 주고 받는 데이터를 확인하지 못하도록 암호화하여 데이터를 주고 받습니다.

# 대칭키 암호화 & 비대칭키 암호화

HTTPS는 두가지 암호화 방식인 대칭키 암호화와 비대칭키 암호화 두가지 방식이 존재합니다.

두 암호화 방식은 서로 대조되는 장단점을 지니고 있습니다.

## 대칭키 암호화

대칭키 암호화는 클라이언트와 서버가 동일한 키를 활용하여

암호화와 복호화를 진행합니다.

대칭키의 장점은 연산속도가 매우 빠르지만

해당 키가 노출되면 매우 위험하다는 단점이 존재합니다.

## 비대칭키 암호화

비대칭키 암호화는 1개의 쌍으로 구성된 공개키와 개인키를

암호화와 복호화를 하는데 사용합니다.

비대칭키의 장점은 키가 노출되어도 비교적 안전하지만

연산 속도가 느리다는 단점이 존재합니다.

### 비대칭키의 암호화 방식

비대칭키 암호화는 공개키와 개인키 암호화 방식을 이용해 데이터를 암호화합니다.

- 공개키 : 모두에게 공개된 키
- 개인키 : 나만 가지고 있는 키

어떤 키로 암호화 또는 복호화를 할지 정함에 따라 서로 다른 효과를 얻을 수 있습니다.

- 공개키로 암호화 + 개인키로 복호화 : 특정 정보를 나만 볼 수 있다.
- 개인키로 암호화 + 공개키로 복호화 : 내가 인증한 정보임을 알려 신뢰성을 보장할 수 있다.

# HTTPS의 작동 방식

HTTPS의 작동 방식은

대칭키와 비대칭키 방식을 전부 사용하는

하이브리드 방식을 통해

통신이 이뤄지게 됩니다.

데이터 전송을 위해 대칭키 방식을 사용하고

해당 대칭키를 안전하게 전달하기 위해

비대칭키 방식을 활용합니다.

## HTTPS 통신 방법

SSL Handshake에 앞서 HTTPS가 TCP 기반의 프로토콜이기 때문에

3-way handshake를 통해 연결 후,

SSL/TLS Handshake를 통해 연결이 진행됩니다.

### SSL/TLS Handshake 과정

1. Client는 Server에 연결을 시도한다 (Client Hello)

   - 자신이 어떤 방식으로 암호화하여 사용할 지 cipher suite에 표시를 합니다.
   - 해당 패킷을 확인해보면 Client Hello라고 출력되는 것을 확인할 수 있다.
     - Cipher Suite
       - 해당 항목에 총 5가지의 알고리즘 채워 넣어 통신을 진행합니다.
         - 키 교환 암호 알고리즘 : Server와 Client간 Key를 교환할 방식을 선정
         - 인증 알고리즘 : Server와 Client간 교환한 인증서를 확인하는 알고리즘
         - 대칭 암호 알고리즘 : 실제 데이터를 암호화하는 알고리즘
         - 블록 암호 운용 방식 : 데이터를 암호화할때 한꺼번에 암호화하는 것이 아닌 블록 단위로 암호화하게 되는데, 블록된 암호화 패킷을 조합하여 데이터를 추측하는 것을 방지하기 위한 방식
         - 해시 알고리즘 : 서로 상대방이 암호화한 것이 맞는지 확인하기 위한 알고리즘

2. Server는 Client에 응답을 보낸다 (Server Hello)
   - 클라이언트로부터 받은 인사(Client Hello)에 대한 응답을 서버로부터 하게 됩니다.
   - 해당 패킷을 확인해보면 Server Hello라고 출력되는 것을 확인할 수 있다.
3. Server는 Client에 Certificate, Server Key Exchange, Server Hello Done을 전송한다.
   - Certificate 패킷 안에는 Server의 SSL 인증서 내용이 들어있다
     - 인증서의 Signature algorithm, public key info 등이 들어있다.
   - Server Key Exchange는 만약 Certificate 패킷 내의 SSL 인증서에 서버의 public key가 없다면 서버가 직접 전달하기 위해 존재한다.
     - 즉, SSL 인증서에 서버의 public key가 존재한다면 Server Key Exchange는 생략된다.
   - 모든 행동을 Server가 마무리했다는 의미로 Server Hello Done을 전송한다.
4. Client에서 Server의 SSL 인증서를 검증합니다.

   - 서버에서 보낸 SSL 인증서에 대한 검증을 하는 단계입니다.

     - SSL 인증서 검증 원리
       1. 클라이언트는 인증서를 발급한 인증기관(CA)의 public key를 찾는다.
       2. 찾은 public key를 이용해 SSL 인증서를 복호화한다.
       3. 복호화 성공 시, 인증서 검증을 완료한다.
     - SSL 인증서를 발급한 CA(인증기관)는 CA의 private key를 통해 인증서를 암호화했다.
     - 그러므로 SSL 인증서는 CA의 public key를 이용해서만 복호화할 수 있다.

     - 클라이언트가 브라우저 또는 안드로이드 기기인 경우 자신이 가지고 있는 리스트에서 확인을 한다.
     - 만약 리스트에 public key가 없다면 외부 인터넷을 통해 인증기관의 정보와 공개키를 가져온다.
       - 그러므로 SSL 인증서를 사용할 때 인터넷 접속이 필요하다.

5. Client는 Server에 대칭키를 전달합니다. (Client Key Exchange, Change Cipher Spec)
   - 클라이언트는 데이터를 암호화하기 위한 대칭키를 생성한다.
   - 클라이언트는 생성한 대칭키를 서버만 볼 수 있게 하기 위해, 서버의 공개키로 암호화해서 서버에 전달한다. (Client Key Exchange)
   - 클라이언트는 전송 완료를 알림 (Change Cipher Spec)
6. Server와 Client의 SSL Handshake가 종료된다.
   - 서버는 클라이언트가 전달한 암호화된 대칭키를 받게된다.
   - 해당 키는 서버에서 비밀키로 복호화해서 얻게 된다.
   - 서버와 클라이언트 모두 동일한 대칭키를 가지고 있기에 통신할 준비가 된다.

## 참고한 자료

- [https://mangkyu.tistory.com/98](https://mangkyu.tistory.com/98)
- [https://devjem.tistory.com/3](https://devjem.tistory.com/3)
- [https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http와-https](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%99%80-https)
- [https://github.com/dongkyun-dev/TIL/blob/master/web/🎭HTTPS.md](https://github.com/dongkyun-dev/TIL/blob/master/web/%F0%9F%8E%ADHTTPS.md)
- [https://brunch.co.kr/@hyoi0303/10](https://brunch.co.kr/@hyoi0303/10)
- [https://nuritech.tistory.com/25](https://nuritech.tistory.com/25)
- [https://run-it.tistory.com/30](https://run-it.tistory.com/30)

### [테크 블로그에 그림과 함께 더 쉽게 풀어쓴 글](https://velog.io/@tnehd1998/HTTP-vs-HTTPS)
