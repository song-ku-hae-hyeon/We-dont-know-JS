# 쿠키 vs 세션 vs JWT

## HTTP의 특징

1. 클라이언트-서버 구조
2. HTTP 메시지(req, res)
3. stateless, connectionless

이 중 stateless에 대해 좀 더 자세하게 다뤄보자.

## Stateless protocol

서버가 클라이언트의 이전 상태를 보존하지 않는다는 의미.

**장점**

-   Visibility : 모든 요청들에 연관성이 없기 때문에 모니터링하기 좋다.
-   Reliability : 일부분이 실패해도 회복하기 쉽다.
-   Scalability : 요청간에 상태를 저장할 필요가 없고 공유할 데이터가 없기 때문에 확장에 문제가 없다.

**단점**

-   반복된 데이터의 전송으로 네트워크 성능을 저하할 수 있다.(매번 사용자를 인증하기 위해 아이디와 비밀번호를 보낸다면?)

## HTTP Cookie

**배경**

HTTP는 stateless 이기 때문에 stateful한 서비스를 제공하기 어렵다.

제공하더라도 불편함을 야기함.

**쿠키란?**

서버가 사용자의 웹 브라우저에 보내는 데이터의 작은 조각이다.

브라우저는 쿠키를 저장해두었다가, 같은 서버로의 다음 요청에 쿠키를 함께 보낸다.

**사용 목적**

1. Session management : 로그인, 장바구니 등
2. Personalization : 사용자 선호도, 테마, 설정 등
3. Tracking : 유저의 행동을 기록, 분석

**장점**

-   정보를 stateful하게 기억해서 사용하기 좋다.

**단점**

-   쿠키를 매 요청의 헤더에 추가해서 보내기 때문에 상당한 트래픽을 발생시킨다.(특히 모바일에서)
-   중요한 데이터를 쿠키에 저장하였을 때 쿠키가 유출되면 보안에 문제가 발생할 수 있다.
-   쉽게 조작할 수 있다.

## Session

**배경**

쿠키의 단점인 트래픽 문제와 쿠키의 보안적 이슈를 해결하기 위해 등장

**세션이란?**

Session id를 식별자로 구별하여 데이터를 사용자의 브라우저에 쿠키형태가 아닌 접속한 서버의 메모리나 DB에 정보를 저장하는 방식.

클라이언트 Session id를 쿠키에 저장해서 가지고 있다.

**장점**

-   서버에 저장하기 때문에 관리하기 편하고 효율적이다.

**단점**

-   load-balancing에서 다루기 어렵다.
-   저장 공간이 부족한 시스템에 적합하지 않다.

## **JWT(JSON Web Token)**

**배경**

session의 단점인 서버에 사용자의 정보를 저장하기 때문에 서버에 부담이 될 수도 있는 이슈를 해결하기 위해 등장

**JWT란?**

JSON 형식을 이용하여 웹에서 사용할 수 있는 엑세스 토큰을 다루는 표준.

**장점**

-   서버에 유저 세션을 유지할 필요가 없다.
-   토큰이 탈취 당하더라도 데이터가 암호화 되어 있어 공격자가 본래 내용을 알 수 없다.

**단점**

-   토큰의 stateless한 특성 때문에 토큰을 강제로 만료시킬 수 없다. → 토큰이 탈취된다면 토큰이 만료될 때까지 서버에 요청을 할 수 있다.

**구조**

모든 JWT는 header, payload, signature 총 3가지로 구성되어 있다.

1. header

-   토큰의 타입
-   해싱 알고리즘 종류

2. payload

-   어떤 데이터가 담겨 있는지(key : value 형식)
-   토큰이 언제까지 유효한지 담으면 좋다.

2. signature

-   데이터와 토큰이 위∙변조 되지 않았음을 증명

## (추가) JWT vs OAuth

둘 다 토큰 기반의 사용자 인증 방식이라고 할 수 있다. 하지만, 몇 가지 다른 점이 있다.

JWT는 의미를 갖는 Claim 기반의 토큰 방식인 반면, OAuth는 아무 의미 없는 문자열 형태이다.

JWT는 제 3의 서버와 의존성이 없지만, OAuth는 제 3의 서버와 의존성이 생긴다.

---

## 출처

[An overview of HTTP - MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#components_of_http-based_systems)

**[HTTP, Stateless, Connectionless, HTTP 메시지 개념](https://velog.io/@duarufp06/HTTP-Stateless-Connectionless-HTTP-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B0%9C%EB%85%90)**

[Stateless protocol - Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)

[Using HTTP cookies - MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

[쿠키(Cookie) 그리고 세션(Session) - Nesoy Blog](https://nesoy.github.io/articles/2017-03/Session-Cookie)

[JWT(JSON Web Token) - 김종근](https://velog.io/@sproutt/JWTJSON-Web-Token-%EA%B9%80%EC%A2%85%EA%B7%BC)

[🙈[HTTP/인증] JWT (JSON Web Token)🐵](https://victorydntmd.tistory.com/115)
