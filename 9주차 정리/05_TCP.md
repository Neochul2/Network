# TCP/IP 네트워크

# PART 5. TCP(Transmission Control Protocol)

> **학습 목표**
>
> - TCP의 역할을 이해한다.
> - TCP가 제공하는 기능을 설명할 수 있다.
> - TCP가 UDP와 다른 이유를 이해한다.
> - TCP가 신뢰성을 제공하는 원리를 이해하기 위한 기초를 학습한다.

---

# 왜 TCP가 필요한가?

앞 장에서는 UDP를 학습했습니다.

UDP는

- 빠르다.
- 단순하다.
- 오버헤드가 작다.

하지만

다음과 같은 문제를 해결하지 않습니다.

- 데이터 손실
- 데이터 순서 변경
- 중복 데이터
- 수신 여부 확인

즉,

UDP는 빠른 전송에는 적합하지만

정확한 전송은 보장하지 않습니다.

이러한 문제를 해결하기 위해

TCP가 만들어졌습니다.

---

# TCP란?

TCP(Transmission Control Protocol)는

전송계층에서 사용하는

연결형(Connection-Oriented)

프로토콜입니다.

TCP의 가장 큰 목적은

**데이터를 정확하고 신뢰성 있게 전달하는 것**입니다.

이를 위해

여러 가지 기능을 함께 사용합니다.

---

# TCP의 특징

TCP는 다음과 같은 특징을 가집니다.

- 연결형(Connection-Oriented)
- 신뢰성(Reliability)
- 순서 보장(In-order Delivery)
- 오류 복구(Error Recovery)
- 양방향 통신(Full Duplex)
- Byte Stream 기반
- 흐름 제어(Flow Control)
- 혼잡 제어(Congestion Control)

이러한 기능들이 함께 동작하여

신뢰성 있는 통신을 제공합니다.

---

# TCP가 제공하는 기능

TCP는 다음과 같은 기능을 제공합니다.

## 1. 연결 생성

통신을 시작하기 전에

송신자와 수신자가 연결을 생성합니다.

---

## 2. 데이터 전송

Application에서 받은 데이터를

TCP Segment로 만들어

IP 계층으로 전달합니다.

```text
Application

↓

TCP

↓

IP

↓

Network
```

---

## 3. 신뢰성 제공

TCP는

데이터가 손실되면

다시 전송합니다.

또한

데이터의 순서를 유지합니다.

---

## 4. 연결 종료

통신이 끝나면

연결을 정상적으로 종료합니다.

---

## 5. 흐름 제어

수신자가 처리 가능한 속도에 맞추어

데이터를 전송합니다.

---

## 6. 혼잡 제어

네트워크가 혼잡하면

전송 속도를 줄여

전체 네트워크 성능을 유지합니다.

---

# TCP가 수행하지 않는 기능

TCP도 모든 기능을 수행하는 것은 아닙니다.

TCP는

- 데이터의 의미를 해석하지 않습니다.
- 파일인지 사진인지 구분하지 않습니다.
- 암호화를 수행하지 않습니다.

TCP는

단순히

Byte Stream을

안전하게 전달하는 역할만 수행합니다.

---

# TCP 동작 개요

```text
Application

↓

TCP Segment 생성

↓

IP Packet 생성

↓

Network

↓

수신

↓

Application
```

---

# TCP는 어떻게 신뢰성을 제공할까?

TCP는

다음과 같은 기능을 함께 사용합니다.

- Sequence Number
- ACK
- Timeout
- Retransmission
- Sliding Window

이 기능들은

다음 장부터

하나씩 자세히 학습합니다.

---

# TCP를 사용하는 서비스

대표적인 TCP 기반 서비스

- HTTP
- HTTPS
- FTP
- SSH
- SMTP
- POP3
- IMAP

이 서비스들은

데이터가 정확하게 전달되어야 합니다.

---

# TCP와 UDP 비교

|항목|TCP|UDP|
|---|---|---|
|연결|연결형|비연결형|
|신뢰성|제공|제공하지 않음|
|재전송|지원|지원하지 않음|
|ACK|사용|사용하지 않음|
|순서 보장|보장|보장하지 않음|
|흐름 제어|지원|지원하지 않음|
|혼잡 제어|지원|지원하지 않음|
|속도|상대적으로 느림|빠름|

---

# 실제 사용 사례

## 웹 브라우징

```text
Web Browser

↓

TCP

↓

Web Server
```

웹 페이지는

HTML, CSS, JavaScript가

정확하게 전달되어야 하므로

TCP를 사용합니다.

---

## 파일 다운로드

```text
사용자

↓

TCP

↓

파일 서버
```

파일이

한 Byte라도 손실되면

정상적으로 사용할 수 없습니다.

따라서 TCP를 사용합니다.

---

# 왜 다음 장이 필요한가?

TCP는

신뢰성을 제공합니다.

그렇다면

어떻게 신뢰성을 제공할까요?

그 첫 번째 비밀은

TCP의 여러 특징에 있습니다.

다음 장에서는

TCP의 대표적인 특성을 학습합니다.

---

# 시험 포인트

- TCP는 연결형 프로토콜이다.
- TCP는 신뢰성을 제공한다.
- TCP는 Byte Stream 기반이다.
- TCP는 흐름 제어와 혼잡 제어를 지원한다.
- TCP는 Application 데이터를 Segment로 캡슐화한다.

---

# 자주 틀리는 부분

❌ TCP는 데이터의 의미를 해석한다.

→ 아니다.

TCP는 Byte Stream만 처리한다.

---

❌ TCP는 암호화를 제공한다.

→ 아니다.

암호화는 TLS/SSL과 같은 상위 계층에서 수행한다.

---

# 핵심 요약

- TCP는 연결형 전송계층 프로토콜이다.
- TCP는 신뢰성 있는 데이터 전송을 제공한다.
- TCP는 연결 생성, 데이터 전송, 연결 종료를 관리한다.
- TCP는 Sequence Number, ACK, Sliding Window 등을 이용하여 신뢰성을 구현한다.

---

# 다음 장

**PART 6. TCP의 특성(TCP Characteristics)**