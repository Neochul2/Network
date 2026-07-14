# TCP/IP 네트워크

# PART 6. TCP의 특성(TCP Characteristics)

> **학습 목표**
>
> - TCP의 대표적인 특성을 이해한다.
> - TCP가 신뢰성을 제공하는 이유를 설명할 수 있다.
> - 이후 학습할 TCP Header, Sequence Number, ACK, Sliding Window의 기초를 이해한다.

---

# 왜 TCP의 특성을 배우는가?

앞 장에서는

TCP가 신뢰성 있는 전송을 제공하는 프로토콜이라는 것을 배웠습니다.

하지만

"신뢰성이 있다."

라는 말만으로는

TCP를 이해했다고 할 수 없습니다.

TCP의 신뢰성은

하나의 기능으로 만들어지는 것이 아니라

여러 기능이 함께 동작하여 만들어집니다.

이번 장에서는

TCP의 대표적인 특성을 학습합니다.

이 장을 이해하면

다음에 배우는

- TCP Header
- Sequence Number
- ACK
- Sliding Window

를 쉽게 이해할 수 있습니다.

---

# TCP의 대표적인 특성

TCP는 다음과 같은 특징을 가집니다.

1. 연결형(Connection-Oriented)

2. 양방향 통신(Full Duplex)

3. 다중화(Multiplexing)

4. 신뢰성(Reliability)

5. 승인(Acknowledgement)

6. Byte Stream 기반(Stream-Oriented)

7. 구조화되지 않은 데이터(Unstructured Byte Stream)

8. 흐름 제어(Flow Control)

이 특성들은

독립적으로 동작하는 것이 아니라

서로 협력하여

신뢰성 있는 통신을 제공합니다.

---

# 1. 연결형(Connection-Oriented)

TCP는

데이터를 전송하기 전에

먼저 연결을 생성합니다.

이를

3-Way Handshake

라고 합니다.

```text
Client

SYN ---------------------->

        <------------------ SYN + ACK

ACK ---------------------->
```

연결이 완료된 후에

데이터를 전송합니다.

UDP는

이러한 과정이 없습니다.

---

## 연결형의 장점

연결을 생성하면서

TCP는

- 상대방이 존재하는지 확인
- 통신 준비
- 초기 정보 교환

을 수행합니다.

이것이

신뢰성의 첫 단계입니다.

---

# 2. 양방향 통신(Full Duplex)

TCP는

송신과 수신을

동시에 수행할 수 있습니다.

예를 들어

```text
Client

HTTP Request

──────────────▶

Server

──────────────▶

HTML Response
```

요청과 응답이

동시에 이루어질 수 있습니다.

---

# 3. 다중화(Multiplexing)

하나의 컴퓨터에서는

여러 프로그램이

동시에 TCP를 사용할 수 있습니다.

예를 들어

```text
Chrome

Discord

Steam

Zoom
```

각 프로그램은

Port 번호를 사용하여

서로 구분됩니다.

---

# 4. 신뢰성(Reliability)

TCP의 가장 중요한 특징입니다.

네트워크에서는

- 데이터 손실
- 데이터 중복
- 데이터 순서 변경

이 발생할 수 있습니다.

TCP는

이 문제를 해결하기 위해

다음 기능을 사용합니다.

- Sequence Number
- ACK
- Retransmission
- Sliding Window

---

# 5. 승인(Acknowledgement)

TCP에서는

수신자가 데이터를 받으면

ACK를 전송합니다.

예를 들어

1~100Byte를 받았다면

```text
ACK = 101
```

을 전송합니다.

즉

101번 Byte부터

보내 달라는 의미입니다.

---

# 6. Byte Stream 기반

TCP는

메시지를 관리하지 않습니다.

Byte들의 연속인

Stream을 관리합니다.

예를 들어

```text
HELLO
```

라는 문자열도

TCP에서는

```text
H

E

L

L

O
```

라는

Byte Stream으로 처리됩니다.

---

# 7. 구조화되지 않은 데이터

TCP는

사진인지

동영상인지

문서인지

구분하지 않습니다.

TCP에게

모든 데이터는

Byte의 연속일 뿐입니다.

즉

데이터의 의미는

Application이 해석합니다.

---

# 8. 흐름 제어(Flow Control)

송신자가

너무 빠르게 데이터를 보내면

수신자가 처리하지 못합니다.

TCP는

Window Size를 이용하여

수신 가능한 양만큼만

데이터를 전송합니다.

이 기능을

Flow Control이라고 합니다.

---

# TCP 특성 요약

|특성|설명|
|---|---|
|연결형|연결 후 데이터 전송|
|양방향|동시 송수신 가능|
|다중화|Port 번호 사용|
|신뢰성|손실·중복·순서 해결|
|ACK|수신 확인|
|Byte Stream|Byte 단위 관리|
|비구조화|데이터 의미를 해석하지 않음|
|흐름 제어|Window Size 사용|

---

# 실제 예시

웹 브라우저에서

웹 서버로 접속하는 경우

```text
Client

↓

TCP 연결

↓

데이터 송수신

↓

연결 종료
```

모든 과정이

TCP의 특성을 이용하여 수행됩니다.

---

# 왜 다음 장이 필요한가?

TCP는

Byte Stream을 관리한다고 배웠습니다.

그렇다면

각 Byte를

어떻게 구분할까요?

또

ACK는

어디에 저장될까요?

그 답은

TCP Header 안에 있습니다.

다음 장에서는

TCP Header의 구조와

각 필드의 역할을 학습합니다.

---

# 시험 포인트

- TCP는 연결형 프로토콜이다.
- TCP는 Full Duplex를 지원한다.
- TCP는 Byte Stream 기반이다.
- TCP는 ACK를 사용한다.
- TCP는 Flow Control을 지원한다.

---

# 자주 틀리는 부분

❌ TCP는 Message를 관리한다.

→ 아니다.

TCP는 Byte Stream을 관리한다.

---

❌ ACK는 마지막으로 받은 Byte 번호이다.

→ 아니다.

ACK는 다음에 받을 Byte 번호이다.

---

# 핵심 요약

- TCP는 여러 기능이 함께 동작하여 신뢰성을 제공한다.
- 연결형, Full Duplex, Byte Stream은 TCP의 대표적인 특징이다.
- ACK와 Flow Control은 TCP의 핵심 기능이다.
- 다음 장에서는 TCP Header를 통해 이러한 기능이 어떻게 구현되는지 학습한다.

---

# 다음 장

**PART 7. TCP 스트림(Stream)과 Sequence Number**