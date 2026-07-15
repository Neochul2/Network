# TCP/IP 네트워크

# PART 7. TCP 스트림(Stream)과 Sequence Number

> **학습 목표**
>
> - TCP의 Byte Stream 개념을 이해한다.
> - TCP Segment와 Byte Stream의 차이를 설명할 수 있다.
> - Sequence Number의 역할을 이해한다.
> - ACK가 Sequence Number를 기반으로 동작하는 이유를 이해한다.

---

# 왜 Stream을 먼저 배울까?

앞 장에서

TCP는 Byte Stream 기반이라고 배웠습니다.

많은 학생들이

여기에서 가장 많이 헷갈립니다.

> TCP는 패킷(Packet)을 보내는데
>
> 왜 Byte를 관리할까?

이 질문을 이해하지 못하면

TCP Header

ACK

Sliding Window

를 모두 암기하게 됩니다.

이번 장에서는

TCP가 데이터를 어떻게 관리하는지부터 이해합니다.

---

# TCP는 메시지를 관리하지 않는다.

우리가 프로그램에서

다음 문자열을 보낸다고 가정합니다.

```text
HELLO WORLD
```

사람은

이것을

하나의 문장이라고 생각합니다.

하지만

TCP는

이 문자열을

메시지(Message)로 생각하지 않습니다.

TCP는

Byte들의 연속으로 생각합니다.

```text
H

E

L

L

O

(공백)

W

O

R

L

D
```

이러한 데이터의 연속을

Byte Stream이라고 합니다.

---

# Stream이란?

Stream은

끊어지지 않는 데이터의 흐름입니다.

예를 들어

수도관에서는

물이 계속 흐릅니다.

```text
████████████████████
```

TCP도

이와 같은 개념입니다.

Byte들이

끊임없이 이어지는 하나의 흐름으로 관리됩니다.

---

# IP는 Stream을 전송할 수 없다.

IP는

Byte Stream을 그대로 전송하지 않습니다.

IP는

Packet 단위로만 데이터를 전송합니다.

따라서

TCP는

Byte Stream을

여러 개의 Segment로 나누어야 합니다.

```text
Application

HELLOWORLD

↓

TCP

HEL

LOW

ORLD

↓

IP
```

이 과정을

Segmentation이라고 합니다.

---

# TCP Segment란?

TCP Segment는

TCP가 만들어 내는

전송 단위입니다.

예를 들어

```text
HELLOWORLD
```

를

다음과 같이 나눌 수 있습니다.

```text
Segment 1

HEL

----------------

Segment 2

LOW

----------------

Segment 3

ORLD
```

중요한 점은

Segment의 크기는

항상 같은 것이 아닙니다.

네트워크 환경에 따라

달라질 수 있습니다.

---

# 순서는 어떻게 알까?

예를 들어

```text
HEL

LOW

ORLD
```

를 전송했는데

```text
LOW
```

Segment가

손실되었다고 가정해 봅시다.

수신자는

어떤 데이터가

빠졌는지

어떻게 알까요?

이를 해결하기 위해

Sequence Number를 사용합니다.

---

# Sequence Number란?

Sequence Number는

각 Byte에 부여되는 번호입니다.

예를 들어

```text
HELLO
```

를 전송하면

```text
Byte

H → 1

E → 2

L → 3

L → 4

O → 5
```

처럼

각 Byte가

고유한 번호를 가집니다.

중요한 점은

Sequence Number는

Segment 번호가 아니라

**Byte 번호**라는 것입니다.

---

# Byte 단위로 관리하는 이유

다음과 같이

15Byte를 전송한다고 가정합니다.

```text
1 ~ 5

6 ~ 10

11 ~ 15
```

그런데

```text
6 ~ 10
```

이 손실되었습니다.

수신자는

```text
1 ~ 5

도착

11 ~ 15

도착
```

을 보고

6번부터

10번까지

없다는 사실을

즉시 알 수 있습니다.

---

# ACK와 Sequence Number

Sequence Number가 있기 때문에

ACK도 가능합니다.

예를 들어

```text
1 ~ 100
```

Byte를 받았다면

수신자는

```text
ACK = 101
```

을 전송합니다.

의미는

```text
101번 Byte부터

계속 보내세요.
```

입니다.

ACK는

Sequence Number가 있기 때문에

동작할 수 있습니다.

---

# 실제 전송 과정

```text
HELLOWORLD

↓

Byte Stream

↓

TCP Segment 생성

↓

Sequence Number 부여

↓

IP 전송

↓

수신

↓

ACK

↓

다음 Byte 전송
```

이 과정이

반복되면서

TCP는

신뢰성 있는 통신을 수행합니다.

---

# 왜 다음 장이 필요한가?

지금까지는

Sequence Number의 개념을 배웠습니다.

그렇다면

이 번호는

도대체 어디에 저장될까요?

정답은

TCP Header입니다.

다음 장에서는

TCP Header의 구조를 살펴보며

Sequence Number

ACK Number

Window

Checksum

Control Bit

등이

어디에 저장되는지 학습합니다.

---

# 시험 포인트

- TCP는 Byte Stream 기반이다.
- TCP는 Byte를 Segment로 분할한다.
- Sequence Number는 Byte 단위 번호이다.
- ACK는 다음에 받을 Byte 번호를 의미한다.
- Sequence Number는 신뢰성의 핵심이다.

---

# 자주 틀리는 부분

❌ Sequence Number는 Segment 번호이다.

→ 아니다.

Byte 번호이다.

---

❌ ACK는 마지막으로 받은 번호이다.

→ 아니다.

다음에 받아야 할 Byte 번호이다.

---

# 핵심 요약

- TCP는 Message가 아니라 Byte Stream을 관리한다.
- Byte Stream은 여러 개의 TCP Segment로 분할된다.
- 모든 Byte에는 Sequence Number가 부여된다.
- ACK는 Sequence Number를 기반으로 동작한다.
- 다음 장에서는 TCP Header 내부 구조를 학습한다.

---

# 다음 장

**PART 8. TCP Header**