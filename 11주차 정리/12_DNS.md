# TCP/IP Advanced

# Chapter 12
# DNS (Domain Name System)

---

# 학습 목표

이번 장에서는 인터넷에서 사용하는 DNS(Domain Name System)를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- DNS가 필요한 이유
- Domain Name과 IP Address의 관계
- DNS Name Space
- DNS 계층 구조
- Name Resolution 과정
- Recursive Query와 Iterative Query
- DNS 동작 과정
- DNS Cache
- Google Public DNS

---

# DNS란?

DNS(Domain Name System)는

사람이 기억하기 쉬운

도메인 이름(Domain Name)을

컴퓨터가 사용하는

IP 주소로 변환해주는 시스템이다.

예)

```
www.google.com

↓

142.250.xxx.xxx
```

사람은

도메인을 기억한다.

컴퓨터는

IP 주소만 이해한다.

DNS는

둘 사이를 연결하는

전화번호부 역할을 한다.

---

# DNS가 필요한 이유

만약 DNS가 없다면

우리는

웹사이트에 접속할 때마다

IP 주소를 직접 입력해야 한다.

예)

```
142.250.206.78
```

하지만

이런 숫자는

외우기 어렵다.

그래서

```
www.google.com
```

처럼

이름을 사용한다.

DNS가

IP 주소를

자동으로 찾아준다.

---

# Domain Name

도메인 이름은

계층 구조로 이루어진다.

예)

```
www.google.com
```

구조

```
www

↓

google

↓

com
```

---

# DNS Name Space

DNS는

트리(Tree) 구조를 사용한다.

```
                .
               Root
          /      |      \
        com      net     org
         |
      google
         |
        www
```

가장 위에는

Root Domain이 있다.

---

# DNS 계층 구조

```
Root

↓

Top Level Domain(TLD)

↓

Second Level Domain

↓

Host Name
```

예)

```
www.google.com
```

Host

↓

www

Domain

↓

google

TLD

↓

com

---

# Top Level Domain(TLD)

대표적인 TLD

```
.com

.net

.org

.edu

.gov

.kr

.jp
```

교수님께서는

국가 도메인도

함께 설명하셨다.

예)

```
co.kr

go.kr

ac.kr
```

---

# DNS Server

DNS는

한 대의 서버가

모든 정보를

저장하지 않는다.

계층적으로

분산 관리한다.

대표적인 서버

- Root DNS
- TLD DNS
- Authoritative DNS
- Local DNS

---

# Name Resolution

Name Resolution은

도메인 이름을

IP 주소로

변환하는 과정이다.

예)

```
www.google.com

↓

DNS Query

↓

IP Address
```

---

# DNS 조회 과정

사용자가

브라우저에서

```
www.google.com
```

을 입력한다.

↓

브라우저는

Local DNS Server에

질문한다.

↓

Local DNS가

모르면

Root DNS에

질문한다.

↓

Root DNS

↓

".com"

DNS를 알려준다.

↓

Local DNS

↓

".com"

DNS에 질문

↓

google.com

DNS를 알려준다.

↓

google.com

DNS에 질문

↓

최종 IP 주소

응답

↓

브라우저 접속

---

# 그림으로 이해하기

```
사용자

↓

Local DNS

↓

Root DNS

↓

.com DNS

↓

google DNS

↓

IP 응답

↓

웹 접속
```

---

# Recursive Query

Recursive Query는

DNS Server에게

"최종 답을 가져와 주세요."

라고 요청하는 방식이다.

사용자는

한 번만

질문하면 된다.

---

# Iterative Query

Iterative Query는

DNS Server가

"나는 모르지만

다음 서버를 물어보세요."

라고 응답하는 방식이다.

Local DNS는

이 과정을 반복하여

최종 IP를 찾는다.

---

# DNS Cache

DNS는

매번

Root부터

조회하지 않는다.

이미 조회한

IP 주소는

Cache에 저장한다.

다음 접속에서는

Cache를

먼저 확인한다.

장점

- 속도 향상
- DNS 부하 감소

---

# TTL(Time To Live)

DNS Cache에는

유효 시간이 있다.

TTL이

끝나면

다시

DNS 조회를 수행한다.

---

# Google Public DNS

대표적인

공용 DNS

```
8.8.8.8

8.8.4.4
```

Cloudflare DNS

```
1.1.1.1
```

---

# 실제 예제

사용자가

```
www.naver.com
```

을 입력한다.

↓

브라우저

↓

Local DNS

↓

IP 조회

↓

223.xxx.xxx.xxx

↓

TCP 연결

↓

HTTP 요청

↓

웹 페이지 표시

DNS는

TCP 연결보다

먼저 수행된다.

---

# 교수님 수업 포인트

★★★★★

DNS는

이름을

IP로

변환한다.

★★★★★

DNS는

계층 구조이다.

★★★★★

Root

↓

TLD

↓

Authoritative

순서로

조회한다.

★★★★★

DNS Cache를

사용한다.

---

# Wireshark에서는?

DNS 패킷은

다음 정보를 확인할 수 있다.

- Query Name
- Query Type(A)
- Response
- TTL
- IP Address

예)

```
Standard query

www.google.com

Type A
```

↓

```
142.250.xxx.xxx
```

---

# 실무 예시

회사 PC에서

웹사이트 접속이

안 된다.

확인 순서

① DNS 응답 확인

↓

② nslookup 실행

↓

③ ping 확인

↓

④ TCP 연결 확인

DNS가

실패하면

웹사이트는

열리지 않는다.

---

# 시험 포인트

★★★★★

✔ DNS는 Domain Name을 IP Address로 변환한다.

✔ DNS는 계층 구조를 사용한다.

✔ Root → TLD → Authoritative 순서로 조회한다.

✔ Recursive Query와 Iterative Query의 차이를 이해한다.

✔ DNS Cache와 TTL의 역할을 이해한다.

---

# 암기법

```
이름

↓

DNS

↓

IP

↓

TCP

↓

HTTP
```

웹 접속은

항상

DNS가 먼저이다.

---

# 면접 질문

Q. DNS란 무엇인가?

Q. DNS가 필요한 이유는?

Q. Recursive Query와 Iterative Query의 차이는?

Q. DNS Cache는 왜 사용하는가?

Q. TTL이란 무엇인가?

---

# 핵심 요약

- DNS는 도메인 이름을 IP 주소로 변환하는 시스템이다.
- DNS는 트리 형태의 계층 구조를 가진다.
- 조회 순서는 Root → TLD → Authoritative DNS이다.
- DNS Cache를 사용하여 조회 속도를 향상시킨다.
- TTL이 지나면 새로운 DNS 조회를 수행한다.
- DNS 조회가 완료된 후 TCP 연결과 HTTP 통신이 시작된다.

---

# 다음 장

Chapter 13에서는

11주차 전체 내용을 정리하는

시험 핵심 정리를 학습한다.

- 교수님 강조 내용
- 핵심 암기
- 빈출 문제
- 예상 서술형
- 면접 질문