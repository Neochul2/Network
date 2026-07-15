# TCP/IP 네트워크

# PART 12. TCP FSM(Finite State Machine)

> **학습 목표**
>
> - TCP FSM(Finite State Machine)의 개념을 이해한다.
> - TCP 연결 과정에서 상태(State)가 어떻게 변하는지 설명할 수 있다.
> - 3-Way Handshake와 4-Way Handshake가 FSM에서 어떻게 표현되는지 이해한다.
> - 각 TCP 상태의 역할을 설명할 수 있다.

---

# 왜 FSM이 필요한가?

앞 장에서는

TCP가

3-Way Handshake와

4-Way Handshake를 이용하여

연결을 생성하고 종료한다는 것을 배웠습니다.

하지만

TCP는

단순히 패킷만 주고받는 것이 아닙니다.

현재

연결이

어떤 상태인지

계속 관리해야 합니다.

예를 들어

- 연결을 기다리는 상태
- 연결을 요청한 상태
- 연결이 완료된 상태
- 연결을 종료하는 상태

등을 구분해야 합니다.

이러한 상태를 관리하기 위해

TCP는

FSM(Finite State Machine)

을 사용합니다.

---

# FSM(Finite State Machine)이란?

FSM은

유한 상태 기계(Finite State Machine)입니다.

현재 상태(State)에 따라

다음 상태가 결정되는 구조입니다.

TCP도

항상 하나의 상태를 가지며

이벤트가 발생할 때마다

다음 상태로 이동합니다.

---

# TCP 상태 전이 개념

```text
현재 상태

↓

이벤트 발생

↓

다음 상태
```

예를 들어

```text
LISTEN

↓

SYN 수신

↓

SYN_RECEIVED

↓

ACK 수신

↓

ESTABLISHED
```

처럼

상태가 계속 변경됩니다.

---

# TCP의 주요 상태

TCP FSM에는

여러 상태가 존재합니다.

대표적인 상태는 다음과 같습니다.

|상태|설명|
|---|---|
|CLOSED|연결이 없는 상태|
|LISTEN|연결 요청을 기다리는 상태|
|SYN_SENT|SYN을 보낸 상태|
|SYN_RECEIVED|SYN을 받고 응답한 상태|
|ESTABLISHED|연결 완료 상태|
|FIN_WAIT_1|FIN을 전송한 상태|
|FIN_WAIT_2|ACK를 받은 상태|
|CLOSE_WAIT|FIN을 받은 상태|
|LAST_ACK|마지막 ACK를 기다리는 상태|
|TIME_WAIT|일정 시간 연결 정보를 유지하는 상태|

---

# CLOSED

```text
CLOSED
```

TCP 연결이

존재하지 않는 상태입니다.

통신을 시작하기 전

또는

연결 종료 후의 상태입니다.

---

# LISTEN

```text
LISTEN
```

Server가

Client의 연결 요청을

기다리는 상태입니다.

웹 서버는

평소 대부분

LISTEN 상태를 유지합니다.

---

# SYN_SENT

Client가

SYN을 전송한 후

응답을 기다리는 상태입니다.

```text
Client

SYN ------------>

SYN_SENT
```

---

# SYN_RECEIVED

Server가

SYN을 받은 후

SYN+ACK를 전송한 상태입니다.

```text
SYN 수신

↓

SYN+ACK 전송

↓

SYN_RECEIVED
```

---

# ESTABLISHED

ACK까지

정상적으로 도착하면

TCP 연결이 완료됩니다.

```text
ESTABLISHED
```

이 상태에서는

실제 데이터를

송수신합니다.

대부분의 TCP 통신은

이 상태에서 이루어집니다.

---

# FIN_WAIT_1

Client가

FIN을 전송하여

연결 종료를 요청한 상태입니다.

```text
FIN 전송

↓

FIN_WAIT_1
```

---

# FIN_WAIT_2

Server가

ACK를 보내면

Client는

FIN_WAIT_2 상태가 됩니다.

이 상태에서는

Server의 FIN을 기다립니다.

---

# CLOSE_WAIT

Server가

Client의 FIN을 받은 상태입니다.

아직

Server가 보낼 데이터가 남아 있을 수 있습니다.

따라서

즉시 연결을 종료하지 않습니다.

---

# LAST_ACK

Server가

FIN을 전송한 후

마지막 ACK를 기다리는 상태입니다.

ACK를 받으면

CLOSED 상태로 이동합니다.

---

# TIME_WAIT

Client가

마지막 ACK를 보낸 후

바로 연결을 삭제하지 않습니다.

일정 시간 동안

연결 정보를 유지합니다.

이를

TIME_WAIT

상태라고 합니다.

---

# 왜 TIME_WAIT가 필요할까?

마지막 ACK가

손실될 수도 있기 때문입니다.

만약

Server가

FIN을 다시 전송하면

Client는

TIME_WAIT 상태에서

ACK를 다시 보낼 수 있습니다.

따라서

TCP는

안전한 연결 종료를 위해

TIME_WAIT를 유지합니다.

---

# TCP FSM 전체 흐름

```text
CLOSED

↓

LISTEN

↓

SYN_RECEIVED

↓

ESTABLISHED

↓

FIN_WAIT_1

↓

FIN_WAIT_2

↓

TIME_WAIT

↓

CLOSED
```

Client와

Server는

각각 다른 상태 전이를 수행하지만

최종적으로

CLOSED 상태가 됩니다.

---

# 3-Way Handshake와 FSM

```text
Client                     Server

CLOSED                     LISTEN

↓

SYN_SENT

---------------------->

                        SYN_RECEIVED

<----------------------

ESTABLISHED

---------------------->

                        ESTABLISHED
```

---

# 4-Way Handshake와 FSM

```text
ESTABLISHED

↓

FIN_WAIT_1

↓

FIN_WAIT_2

↓

TIME_WAIT

↓

CLOSED
```

Server는

```text
ESTABLISHED

↓

CLOSE_WAIT

↓

LAST_ACK

↓

CLOSED
```

순으로 이동합니다.

---

# FSM이 중요한 이유

FSM은

현재 TCP 연결이

어떤 상태인지

정확하게 관리합니다.

이를 통해

연결 생성

데이터 송수신

연결 종료

과정이

안전하게 수행됩니다.

---

# 왜 다음 장이 필요한가?

지금까지

TCP/IP 전송계층의

모든 핵심 개념을 학습했습니다.

마지막 장에서는

시험에 자주 출제되는 내용을

한 번에 정리하고,

TCP와 UDP를 비교하며

전체 내용을 복습합니다.

---

# 시험 포인트

- TCP는 FSM으로 연결 상태를 관리한다.
- LISTEN은 연결 요청을 기다리는 상태이다.
- ESTABLISHED는 데이터 송수신 상태이다.
- TIME_WAIT는 안전한 연결 종료를 위해 존재한다.
- CLOSED는 연결이 없는 상태이다.

---

# 자주 틀리는 부분

❌ TIME_WAIT는 오류 상태이다.

→ 아니다.

정상적인 연결 종료 과정의 일부이다.

---

❌ LISTEN은 Client 상태이다.

→ 아니다.

일반적으로 Server가 연결 요청을 기다리는 상태이다.

---

# 핵심 요약

- TCP는 FSM을 이용하여 연결 상태를 관리한다.
- 연결 생성과 종료 과정은 상태 전이(State Transition)로 표현된다.
- ESTABLISHED는 실제 데이터 송수신이 이루어지는 상태이다.
- TIME_WAIT는 마지막 ACK의 신뢰성을 보장하기 위해 존재한다.
- FSM은 TCP 연결 관리의 핵심 메커니즘이다.

---

# 다음 장

**PART 13. 시험 핵심정리**