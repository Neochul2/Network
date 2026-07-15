# TCP/IP Advanced

# Chapter 10
# Flow Control

---

# 학습 목표

이번 장에서는 TCP의 흐름 제어(Flow Control)를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- Flow Control이 필요한 이유
- Receiver Buffer의 역할
- Window Size 조절
- Zero Window
- Window Update
- Sliding Window와 Flow Control의 관계

---

# Flow Control이란?

Flow Control(흐름 제어)은

수신자가 처리할 수 있는 속도에 맞추어

송신자의 전송 속도를 조절하는 기능이다.

즉,

> **"상대방이 받을 수 있는 만큼만 보내는 기술"**

이다.

---

# 왜 Flow Control이 필요한가?

송신자가

매우 빠른 속도로 데이터를 보낸다고 가정하자.

```text
송신자

100MB/s

↓

수신자

10MB/s
```

수신자는

들어오는 데이터를

모두 처리하지 못한다.

결국

버퍼(Buffer)가 가득 차고

데이터가 손실될 수 있다.

TCP는 이런 상황을 막기 위해

Flow Control을 사용한다.

---

# Receiver Buffer란?

수신자는

도착한 데이터를

바로 애플리케이션에 전달하지 않는다.

먼저

메모리(Buffer)에 저장한다.

```text
TCP

↓

Receiver Buffer

↓

Application
```

즉,

Buffer는

잠시 데이터를 보관하는 공간이다.

---

# Window Size와 Buffer의 관계

TCP의 Window Size는

수신 버퍼의 남은 공간을 의미한다.

예를 들어

버퍼 크기

```
1000Byte
```

현재 사용량

```
400Byte
```

남은 공간

```
600Byte
```

↓

TCP Header

```
Window = 600
```

송신자는

최대

600Byte까지만

추가로 전송한다.

---

# Buffer가 가득 차면?

예를 들어

```text
Buffer

1000Byte
```

↓

현재

1000Byte 사용

↓

남은 공간

0Byte

그러면

TCP Header에는

```text
Window = 0
```

이 기록된다.

이를

**Zero Window**

라고 한다.

---

# Zero Window란?

Zero Window는

수신자가

더 이상 데이터를 받을 수 없는 상태이다.

```text
Window = 0

↓

송신 중지
```

송신자는

새로운 데이터를 보내지 않고

기다린다.

---

# Window Update

수신 애플리케이션이

버퍼의 데이터를 처리하면

버퍼에

공간이 생긴다.

예를 들어

```text
Window = 0

↓

애플리케이션 처리

↓

Window = 500
```

수신자는

새로운 Window Size를

송신자에게 알려준다.

이를

**Window Update**

라고 한다.

---

# Flow Control 동작 과정

### Step 1

송신자

데이터 전송

↓

### Step 2

수신자

Buffer 저장

↓

### Step 3

Window Size 계산

↓

### Step 4

TCP Header의 Window 필드에 기록

↓

### Step 5

송신자는

Window 크기만큼만

전송

↓

### Step 6

Buffer가 가득 차면

Zero Window

↓

### Step 7

버퍼가 비워지면

Window Update

↓

전송 재개

---

# Sliding Window와의 관계

많은 학생들이

Sliding Window와

Flow Control을

다른 기술이라고 생각한다.

하지만

Sliding Window는

Flow Control을 구현하는 방법이다.

즉,

```text
Flow Control

↓

Sliding Window

↓

Window Size 조절
```

이 관계를 이해해야 한다.

---

# 교수님 수업 포인트

교수님께서는

다음 내용을 강조하셨다.

★★★★★

Window

=

Receiver Buffer

★★★★★

Buffer가

가득 차면

Window는

작아진다.

★★★★★

Window가

0이면

송신자는

전송을 멈춘다.

---

# Wireshark에서는?

TCP Header의

Window Size

항목을 보면

현재 수신자가

얼마나 더 받을 수 있는지

확인할 수 있다.

예)

```text
Window Size = 8192
```

또는

```text
Window Size = 0
```

---

# 실무 예시

사용자가

대용량 영상을 다운로드하고 있다.

하지만

PC의 CPU가 매우 바빠

애플리케이션이

데이터를 천천히 읽는다.

그러면

Receiver Buffer가

점점 가득 찬다.

↓

TCP는

Window Size를

점점 줄인다.

↓

송신자는

전송 속도를 낮춘다.

↓

버퍼가 비워지면

Window Size를

다시 증가시킨다.

---

# Flow Control과 Congestion Control의 차이

많은 학생들이

헷갈리는 부분이다.

Flow Control

↓

수신자의 처리 속도를 고려한다.

Congestion Control

↓

네트워크 자체의 혼잡을 고려한다.

즉

Flow Control은

상대 컴퓨터를 보호하는 기술이다.

Congestion Control은

네트워크를 보호하는 기술이다.

---

# 시험 포인트

★★★★★

✔ Flow Control은 수신자의 처리 속도에 맞추어 송신 속도를 조절한다.

✔ Window Size는 Receiver Buffer의 남은 공간이다.

✔ Window = 0 이면 Zero Window이다.

✔ Sliding Window는 Flow Control을 구현하는 기술이다.

---

# 암기법

```text
Buffer 감소

↓

Window 감소

↓

전송 속도 감소

------------------

Buffer 증가

↓

Window 증가

↓

전송 속도 증가
```

---

# 면접 질문

Q. Flow Control이란 무엇인가?

Q. Window Size는 무엇을 의미하는가?

Q. Zero Window란 무엇인가?

Q. Sliding Window와 Flow Control의 관계를 설명하시오.

Q. Flow Control과 Congestion Control의 차이는?

---

# 핵심 요약

- Flow Control은 수신자의 처리 능력을 보호하는 기술이다.
- Window Size는 Receiver Buffer의 남은 공간을 의미한다.
- Window가 0이 되면 송신자는 데이터를 보내지 않는다.
- 버퍼에 공간이 생기면 Window Update를 통해 전송을 다시 시작한다.
- Sliding Window는 Flow Control을 구현하는 핵심 메커니즘이다.

---

# 다음 장

Chapter 11에서는

Silly Window Syndrome(SWS)을 학습한다.

- SWS란?
- 왜 발생하는가?
- 성능이 왜 나빠지는가?
- 해결 방법