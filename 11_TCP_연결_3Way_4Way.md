# TCP/IP 네트워크

# PART 11. TCP 연결(3-Way Handshake / 4-Way Handshake)

> **학습 목표**
>
> - TCP가 연결형(Connection-Oriented) 프로토콜인 이유를 이해한다.
> - 3-Way Handshake의 동작 과정을 설명할 수 있다.
> - 4-Way Handshake의 동작 과정을 설명할 수 있다.
> - SYN, ACK, FIN 플래그의 역할을 이해한다.

---

# 왜 연결(Connection)이 필요한가?

앞 장에서는

TCP가

신뢰성 있는 데이터 전송을 제공한다는 것을 배웠습니다.

하지만

데이터를 보내기 전에

먼저 확인해야 할 것이 있습니다.

- 상대방이 존재하는가?
- 데이터를 받을 준비가 되었는가?
- 초기 Sequence Number는 무엇인가?

이러한 정보를 서로 확인하기 위해

TCP는

데이터를 보내기 전에

먼저 연결(Connection)을 생성합니다.

---

# TCP는 연결형(Connection-Oriented) 프로토콜

TCP는

데이터를 전송하기 전에

반드시 연결을 생성합니다.

그리고

통신이 끝나면

연결을 종료합니다.

즉,

TCP 통신은

다음과 같은 순서로 이루어집니다.

```text
연결 생성

↓

데이터 전송

↓

연결 종료
```

---

# TCP 연결 생성

TCP 연결 생성은

3-Way Handshake를 이용합니다.

Handshake란

서로 통신이 가능한지 확인하는 과정입니다.

TCP는

세 번의 메시지를 교환하여

연결을 생성합니다.

---

# 3-Way Handshake

```text
Client                          Server

SYN --------------------------->

        <---------------------- SYN + ACK

ACK --------------------------->
```

총 3번의 패킷을 교환하므로

3-Way Handshake라고 합니다.

---

# 1단계 : SYN

Client는

Server에게

연결을 요청합니다.

이때

TCP Header의

SYN 비트를 1로 설정합니다.

```text
SYN = 1
```

의미는

```text
연결을 시작하겠습니다.
```

입니다.

---

# 2단계 : SYN + ACK

Server는

연결 요청을 수락합니다.

그리고

자신도 연결할 준비가 되었음을 알립니다.

```text
SYN = 1

ACK = 1
```

즉,

```text
연결 요청을 받았습니다.

저도 준비되었습니다.
```

라는 의미입니다.

---

# 3단계 : ACK

Client는

Server의 응답을 확인합니다.

그리고

마지막 ACK를 전송합니다.

```text
ACK = 1
```

이 메시지가 도착하면

TCP 연결이 완료됩니다.

---

# 왜 세 번이나 통신할까?

많은 학생들이

다음과 같은 질문을 합니다.

> 두 번만 통신하면 안 될까?

안 됩니다.

Server도

Client가

자신의 응답을

정상적으로 받았는지

확인해야 하기 때문입니다.

세 번째 ACK가 있어야

양쪽 모두

연결이 성공했음을 확인할 수 있습니다.

---

# 연결이 완료되면

3-Way Handshake가 끝나면

비로소

데이터를 전송할 수 있습니다.

```text
Client

──────────────▶

Data

──────────────▶

Server
```

---

# TCP 연결 종료

통신이 끝났다고 해서

갑자기 연결을 끊으면 안 됩니다.

아직

상대방이 보낼 데이터가

남아 있을 수도 있기 때문입니다.

TCP는

안전하게 연결을 종료하기 위해

4-Way Handshake를 사용합니다.

---

# 4-Way Handshake

```text
Client                          Server

FIN --------------------------->

        <---------------------- ACK

        <---------------------- FIN

ACK --------------------------->
```

총 네 번의 메시지를 교환합니다.

---

# 1단계 : FIN

Client는

더 이상

보낼 데이터가 없음을 알립니다.

```text
FIN = 1
```

---

# 2단계 : ACK

Server는

FIN을 정상적으로 받았음을 알립니다.

```text
ACK = 1
```

하지만

Server는

아직 보낼 데이터가

남아 있을 수 있습니다.

따라서

즉시 연결을 종료하지 않습니다.

---

# 3단계 : FIN

Server도

모든 데이터 전송을 완료하면

FIN을 보냅니다.

```text
FIN = 1
```

의미는

```text
저도 보낼 데이터가 없습니다.
```

입니다.

---

# 4단계 : ACK

Client는

Server의 FIN을 확인하고

마지막 ACK를 전송합니다.

```text
ACK = 1
```

이후

TCP 연결이 종료됩니다.

---

# TCP Control Bit 정리

|Control Bit|역할|
|---|---|
|SYN|연결 생성|
|ACK|수신 확인|
|FIN|정상 연결 종료|
|RST|비정상 연결 종료|
|PSH|응용프로그램에 즉시 전달|
|URG|긴급 데이터 표시|

---

# 3-Way와 4-Way 비교

|구분|3-Way Handshake|4-Way Handshake|
|---|---|---|
|목적|연결 생성|연결 종료|
|메시지 수|3개|4개|
|대표 Flag|SYN, ACK|FIN, ACK|

---

# 실제 웹 브라우저 통신

웹 브라우저에서

웹 서버에 접속하면

다음과 같은 과정이 수행됩니다.

```text
3-Way Handshake

↓

HTTP Request

↓

HTTP Response

↓

4-Way Handshake
```

웹 브라우저가

홈페이지 하나를 열 때도

이 과정이 반복됩니다.

---

# 왜 다음 장이 필요한가?

지금까지는

TCP 연결 과정을 배웠습니다.

하지만

연결이 생성되는 동안

TCP는

여러 상태(State)를 거칩니다.

예를 들어

- LISTEN
- SYN_SENT
- SYN_RECEIVED
- ESTABLISHED
- FIN_WAIT
- TIME_WAIT

등의 상태가 존재합니다.

이러한 상태를

FSM(Finite State Machine)

이라고 합니다.

다음 장에서는

TCP 상태 전이 과정을

FSM을 이용하여 학습합니다.

---

# 시험 포인트

- TCP는 연결형 프로토콜이다.
- 연결 생성은 3-Way Handshake를 사용한다.
- 연결 종료는 4-Way Handshake를 사용한다.
- SYN은 연결 생성에 사용된다.
- FIN은 연결 종료에 사용된다.
- ACK는 수신 확인에 사용된다.

---

# 자주 틀리는 부분

❌ TCP 연결 종료도 3-Way Handshake를 사용한다.

→ 아니다.

연결 종료는 4-Way Handshake를 사용한다.

---

❌ FIN을 보내면 즉시 연결이 종료된다.

→ 아니다.

상대방도 FIN을 보내야 정상적으로 연결이 종료된다.

---

# 핵심 요약

- TCP는 데이터를 보내기 전에 3-Way Handshake로 연결을 생성한다.
- 연결이 완료된 후 데이터를 전송한다.
- 통신이 끝나면 4-Way Handshake로 안전하게 연결을 종료한다.
- SYN, ACK, FIN은 TCP 연결 제어의 핵심 Control Bit이다.

---

# 다음 장

**PART 12. TCP FSM(Finite State Machine)**