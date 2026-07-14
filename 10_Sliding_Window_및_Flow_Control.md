# TCP/IP 네트워크

# PART 10. Sliding Window 및 Flow Control

> **학습 목표**
>
> - Sliding Window의 개념을 이해한다.
> - Flow Control이 필요한 이유를 이해한다.
> - Window Size의 역할을 설명할 수 있다.
> - TCP가 데이터를 연속적으로 전송하는 원리를 이해한다.

---

# 왜 Sliding Window가 필요한가?

앞 장에서는

TCP가

ACK와 Retransmission을 이용하여

신뢰성을 제공하는 방법을 배웠습니다.

하지만

아직 해결되지 않은 문제가 있습니다.

송신자가

무한정 빠르게 데이터를 보내면

수신자는

모든 데이터를 처리할 수 있을까요?

답은

그렇지 않습니다.

수신자의 처리 속도보다

더 많은 데이터가 도착하면

버퍼(Buffer)가 가득 차고

데이터가 손실될 수 있습니다.

이 문제를 해결하기 위해

TCP는

Flow Control과

Sliding Window를 사용합니다.

---

# Flow Control이란?

Flow Control은

송신자의 전송 속도를

수신자의 처리 속도에 맞추는 기능입니다.

즉,

수신자가

받을 수 있는 만큼만

데이터를 전송하도록 조절합니다.

---

# 수신 버퍼(Buffer)

수신자는

도착한 데이터를

즉시 처리하지 않습니다.

먼저

메모리(Buffer)에 저장한 후

순서대로 처리합니다.

예를 들어

버퍼 크기가

8Byte라면

```text
┌─────────────────┐

□□□□□□□□

└─────────────────┘

8 Byte
```

송신자가

20Byte를 한 번에 보내면

버퍼가 넘쳐

데이터가 손실될 수 있습니다.

---

# Window란?

Window는

한 번에

ACK를 기다리지 않고

전송할 수 있는 데이터의 범위를 의미합니다.

예를 들어

Window Size가

4Byte라면

```text
1

2

3

4
```

까지는

ACK를 기다리지 않고

연속으로 전송할 수 있습니다.

---

# Sliding Window란?

Sliding Window는

ACK를 받을 때마다

전송 가능한 영역(Window)이

앞으로 이동하는 방식입니다.

예를 들어

Window Size가

4라고 가정합니다.

초기 상태

```text
송신 가능

[1][2][3][4]

대기

[5][6][7][8]
```

---

# ACK를 받으면

1~4Byte를

정상적으로 전송한 후

ACK를 받으면

Window가 앞으로 이동합니다.

```text
전송 완료

[1][2][3][4]

송신 가능

[5][6][7][8]
```

이처럼

Window가

앞으로 미끄러지듯 이동한다고 하여

Sliding Window라고 합니다.

---

# Sliding Window 동작 과정

```text
송신

1

2

3

4

──────────────▶

수신

↓

ACK = 5

<──────────────

Window 이동

5

6

7

8

──────────────▶
```

이 과정을 반복하여

끊김 없이 데이터를 전송합니다.

---

# Window Size

Window Size는

수신자가

현재 받을 수 있는 데이터의 크기를 의미합니다.

TCP Header의

Window Size 필드에 저장됩니다.

예를 들어

```text
Window Size

8192 Byte
```

라면

송신자는

최대 8192Byte까지

ACK를 기다리지 않고

전송할 수 있습니다.

---

# Window Size가 0인 경우

수신 버퍼가

가득 차면

수신자는

```text
Window = 0
```

을 송신자에게 알립니다.

송신자는

데이터 전송을 일시적으로 중단합니다.

버퍼에

여유 공간이 생기면

새로운 Window Size를 알려주고

전송을 다시 시작합니다.

---

# Stop-and-Wait와의 비교

Sliding Window를 이해하려면

Stop-and-Wait 방식과 비교하면 쉽습니다.

### Stop-and-Wait

```text
1 전송

↓

ACK 대기

↓

2 전송

↓

ACK 대기

↓

3 전송
```

매번

ACK를 기다려야 하므로

속도가 느립니다.

---

### Sliding Window

```text
1

2

3

4

연속 전송

↓

ACK

↓

5

6

7

8

연속 전송
```

전송 효율이

매우 높습니다.

---

# Sliding Window의 장점

- ACK를 기다리는 시간을 줄인다.
- 네트워크 대역폭을 효율적으로 사용한다.
- 전송 속도를 높인다.
- 수신자의 처리 속도를 초과하지 않는다.

즉

신뢰성과

성능을

동시에 향상시킵니다.

---

# Flow Control과 Congestion Control의 차이

많은 학생들이

두 개념을 혼동합니다.

|구분|Flow Control|Congestion Control|
|---|---|---|
|대상|수신자|네트워크|
|목적|수신자 보호|네트워크 보호|
|기준|Receiver Buffer|Network 상태|

Flow Control은

수신자가 처리하지 못하는 상황을 막기 위한 기능입니다.

Congestion Control은

네트워크 전체가 혼잡해지는 것을 막기 위한 기능입니다.

---

# 실제 예시

수신자의 버퍼가

다음과 같다고 가정합니다.

```text
Window Size

4096 Byte
```

송신자는

4096Byte까지

연속 전송합니다.

수신자가

ACK와 함께

새로운 Window Size를

8192Byte로 알려주면

송신자는

8192Byte까지

연속 전송할 수 있습니다.

---

# 왜 다음 장이 필요한가?

지금까지는

데이터를

정확하고 효율적으로

전송하는 방법을 배웠습니다.

하지만

TCP는

데이터를 보내기 전에

먼저 연결을 생성해야 합니다.

또한

통신이 끝나면

정상적으로 연결을 종료해야 합니다.

다음 장에서는

3-Way Handshake와

4-Way Handshake를 이용한

TCP 연결 과정을 학습합니다.

---

# 시험 포인트

- Sliding Window는 연속 전송 기법이다.
- Flow Control은 수신자의 처리 속도를 고려한다.
- Window Size는 TCP Header에 저장된다.
- ACK를 받으면 Window가 앞으로 이동한다.
- Window Size가 0이면 송신은 일시 중단된다.
- Flow Control과 Congestion Control의 차이를 구분할 수 있어야 한다.

---

# 자주 틀리는 부분

❌ Sliding Window는 오류를 검출하는 기능이다.

→ 아니다.

전송 효율과 흐름 제어를 위한 기법이다.

---

❌ Flow Control과 Congestion Control은 같은 개념이다.

→ 아니다.

Flow Control은 수신자를 보호하고,

Congestion Control은 네트워크를 보호한다.

---

# 핵심 요약

- Sliding Window는 ACK를 기다리지 않고 여러 데이터를 연속으로 전송하는 기법이다.
- Window Size는 한 번에 전송 가능한 데이터의 양을 의미한다.
- ACK를 받을 때마다 Window가 앞으로 이동한다.
- Flow Control은 수신자의 처리 속도에 맞추어 데이터를 전송한다.
- Flow Control과 Congestion Control은 목적이 서로 다르다.

---

# 다음 장

**PART 11. TCP 연결(3-Way Handshake / 4-Way Handshake)**