# TCP/IP 네트워크

# PART 9. TCP 신뢰성(Reliability) - ACK, Timeout, Retransmission, PAR

> **학습 목표**
>
> - TCP가 신뢰성을 제공하는 원리를 이해한다.
> - ACK의 동작 과정을 설명할 수 있다.
> - Timeout과 Retransmission의 관계를 이해한다.
> - PAR(Positive Acknowledgment with Retransmission)의 동작 원리를 이해한다.

---

# 왜 TCP는 신뢰성이 높을까?

앞 장에서는

TCP Header 안에

Sequence Number와

ACK Number가 저장된다는 것을 배웠습니다.

하지만

번호만 저장한다고 해서

신뢰성이 생기는 것은 아닙니다.

TCP는

Sequence Number

ACK

Timeout

Retransmission

이 네 가지 기능을 함께 사용하여

신뢰성 있는 통신을 제공합니다.

이러한 방식을

PAR(Positive Acknowledgment with Retransmission)

이라고 합니다.

---

# 신뢰성이란?

신뢰성(Reliability)이란

송신한 데이터가

손실되지 않고,

순서대로,

중복 없이

수신자에게 전달되는 것을 의미합니다.

TCP는

이 목표를 달성하기 위해

지속적으로

데이터를 확인하고

필요하면 다시 전송합니다.

---

# Sequence Number의 역할

모든 Byte에는

고유한 번호가 부여됩니다.

예를 들어

```text
Byte

1

2

3

4

5

6

7

8
```

수신자는

이 번호를 이용하여

데이터의 순서를 확인합니다.

또한

손실된 데이터도

쉽게 발견할 수 있습니다.

---

# ACK(Acknowledgment)

ACK는

수신자가

데이터를 정상적으로 받았다는 것을

송신자에게 알려주는 메시지입니다.

예를 들어

1~100Byte를 받았다면

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

마지막으로 받은 번호가 아니라

다음에 받을 번호를 의미합니다.

---

# ACK 동작 과정

```text
Client

1 ~ 100 Byte

---------------------------->

Server

ACK = 101

<----------------------------
```

송신자는

ACK를 받으면

다음 데이터를 계속 전송합니다.

---

# Timeout

만약

ACK가 오지 않는다면

송신자는

상대방이

데이터를 받지 못했다고 판단합니다.

하지만

바로 재전송하지는 않습니다.

일정 시간 동안

기다립니다.

이 시간을

Timeout이라고 합니다.

---

# Retransmission

Timeout 시간이 지나도

ACK가 오지 않으면

송신자는

같은 데이터를

다시 전송합니다.

이를

Retransmission

(재전송)

이라고 합니다.

---

# 재전송 과정

```text
Client

1 ~ 100

---------------------------->

(손실)

XXXXXXXXXXXXXXXXXXXXXXXXXXXXX

ACK 없음

↓

Timeout 발생

↓

1 ~ 100 재전송

---------------------------->

Server

ACK = 101

<----------------------------
```

이 과정을 통해

손실된 데이터도

복구할 수 있습니다.

---

# Duplicate ACK

다음과 같은 상황을 생각해 봅시다.

```text
전송

1~100

101~200

201~300
```

그런데

```text
101~200
```

이 손실되었습니다.

수신자는

```text
1~100

도착

↓

ACK=101

↓

201~300 도착

↓

여전히

ACK=101
```

을 계속 보냅니다.

이를

Duplicate ACK

라고 합니다.

송신자는

같은 ACK가 반복되면

중간 데이터가

손실되었다고 판단합니다.

---

# Fast Retransmission

TCP는

Timeout을 기다리지 않고

같은 ACK가

3번 반복되면

즉시 재전송합니다.

이를

Fast Retransmission

이라고 합니다.

```text
ACK=101

ACK=101

ACK=101

↓

즉시 재전송
```

이 기능은

네트워크 성능을 크게 향상시킵니다.

---

# PAR(Positive Acknowledgment with Retransmission)

TCP의 신뢰성은

PAR 방식으로 구현됩니다.

PAR는

두 가지 원리를 사용합니다.

1.

Positive Acknowledgment

↓

데이터를 받으면

ACK를 보냅니다.

2.

Retransmission

↓

ACK가 오지 않으면

다시 보냅니다.

즉

```text
전송

↓

ACK 확인

↓

ACK 도착

↓

다음 데이터

------------------

ACK 없음

↓

Timeout

↓

재전송
```

이 과정을

반복하면서

신뢰성을 제공합니다.

---

# TCP 신뢰성 전체 과정

```text
Sequence Number 생성

↓

데이터 전송

↓

수신

↓

ACK 전송

↓

ACK 확인

↓

다음 데이터

↓

ACK 없음

↓

Timeout

↓

Retransmission
```

이 과정이

계속 반복됩니다.

---

# TCP가 신뢰성을 제공하는 이유

TCP는

단순히 데이터를 보내는 것이 아닙니다.

전송 후에도

상대방의 응답을 계속 확인합니다.

따라서

데이터 손실이 발생해도

복구가 가능합니다.

이것이

UDP와 가장 큰 차이입니다.

---

# 왜 다음 장이 필요한가?

지금까지는

데이터를

정확하게 보내는 방법을 배웠습니다.

하지만

송신자가

너무 빠르게 데이터를 보내면

수신자가 처리하지 못합니다.

이를 해결하기 위해

TCP는

Sliding Window와

Flow Control을 사용합니다.

다음 장에서는

Sliding Window의 동작 원리를 학습합니다.

---

# 시험 포인트

- ACK는 다음에 받을 Byte 번호를 의미한다.
- ACK가 오지 않으면 Timeout 후 재전송한다.
- Retransmission은 재전송 기능이다.
- PAR는 Positive Acknowledgment with Retransmission의 약자이다.
- Duplicate ACK는 손실을 빠르게 발견하는 방법이다.
- Fast Retransmission은 Duplicate ACK 3회 후 즉시 재전송한다.

---

# 자주 틀리는 부분

❌ ACK는 마지막으로 받은 Byte 번호이다.

→ 아니다.

다음에 받아야 할 Byte 번호이다.

---

❌ TCP는 항상 Timeout 후에만 재전송한다.

→ 아니다.

Duplicate ACK가 3번 발생하면

Fast Retransmission을 수행한다.

---

❌ PAR는 별도의 프로토콜이다.

→ 아니다.

TCP가 사용하는 신뢰성 전송 방식이다.

---

# 핵심 요약

- TCP는 Sequence Number와 ACK를 이용하여 데이터의 순서를 관리한다.
- ACK가 오지 않으면 Timeout 후 Retransmission을 수행한다.
- Duplicate ACK를 이용하여 손실을 빠르게 발견할 수 있다.
- Fast Retransmission은 Timeout을 기다리지 않고 재전송한다.
- TCP의 신뢰성은 PAR 방식으로 구현된다.

---

# 다음 장

**PART 10. Sliding Window 및 Flow Control**