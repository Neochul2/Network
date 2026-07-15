# TCP/IP Advanced

# Chapter 02
# TCP Header 필드 완전정리

---

# 학습 목표

이번 장에서는 TCP Header를 구성하는 모든 필드의 역할과 동작 원리를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- TCP Header 구조
- 각 필드의 역할
- 실제 통신에서 사용하는 값
- Wireshark에서 확인하는 방법
- 시험에 자주 출제되는 Header 필드

---

# TCP Header란?

TCP Header는 TCP가 데이터를 신뢰성 있게 전송하기 위해 사용하는 제어정보의 집합이다.

TCP Segment는

```text
TCP Header

+

Data(Payload)
```

로 구성된다.

Header에는

- 연결 관리
- 순서 관리
- 오류 검출
- 흐름 제어

를 위한 정보가 저장된다.

---

# TCP Header 구조

```text
0                   15 16                  31
+----------------------+----------------------+
| Source Port          | Destination Port     |
+----------------------+----------------------+
| Sequence Number                             |
+---------------------------------------------+
| Acknowledgement Number                      |
+---------------------------------------------+
| Data Offset | Reserved | Control Bits       |
+----------------------+----------------------+
| Window               | Checksum             |
+----------------------+----------------------+
| Urgent Pointer                             |
+---------------------------------------------+
| Options (Variable)                         |
+---------------------------------------------+
| Padding                                   |
+---------------------------------------------+
| Data                                      |
+---------------------------------------------+
```

---

# Source Port

## 정의

세그먼트를 송신하는 프로그램의 포트 번호이다.

크기

```
16bit
```

예)

```
52341
```

웹 브라우저는

임시(Ephemeral) Port를 사용한다.

---

## 실제 예

Chrome

↓

Source Port

52341

↓

Destination Port

443

↓

HTTPS 서버

---

## 왜 필요한가?

응답을 받을 프로그램을 구분하기 위해서이다.

하나의 PC에서

브라우저

게임

메신저

가 동시에 TCP를 사용할 수 있기 때문이다.

---

## 시험 포인트

클라이언트는 대부분

Ephemeral Port를 사용한다.

---

# Destination Port

목적지 프로그램의 Port 번호이다.

웹 서버라면

```
80

443
```

메일 서버

```
25

587
```

SSH

```
22
```

등이 된다.

---

## Source Port와 Destination Port 비교

|항목|Source|Destination|
|----|------|-----------|
|의미|출발 프로그램|도착 프로그램|
|브라우저|52341|443|
|응답|443|52341|

---

# Sequence Number

## 정의

TCP가 보내는

첫 번째 Byte의 번호이다.

TCP는

Byte마다 번호를 붙인다.

---

예

```
Hello
```

↓

```
H

e

l

l

o
```

↓

```
100

101

102

103

104
```

Sequence Number

=

100

---

## 왜 필요한가?

순서를 맞추기 위해

재전송을 위해

중복 제거를 위해

---

교수님 강조

★★★★★

가장 중요

---

# ACK Number

수신자가

다음에 받고 싶은 Byte 번호

예

```
1~100

수신 완료
```

↓

ACK

101

---

ACK는

100번을 받았다

가 아니라

101번부터 보내라

이다.

---

교수님 강조

★★★★★

---

# Data Offset

TCP Header 길이

단위

```
32bit Word
```

예

```
5

↓

20Byte Header
```

교수님 말씀

> Data Offset은 시험 비중이 낮다.

---

# Reserved

미래를 위해 예약

항상

0

---

# Control Bits

TCP 제어 기능

- URG
- ACK
- PSH
- RST
- SYN
- FIN

각 비트는

3Way Handshake

종료

Reset

등에 사용된다.

다음 장에서 다시 학습한다.

---

# Window

수신 가능한 버퍼 크기

Flow Control의 핵심

★★★★★

교수님이 가장 많이 설명한 필드 중 하나

---

# Checksum

오류 검출

TCP는

Pseudo Header까지 포함해서

Checksum 계산

★★★★★

---

# Urgent Pointer

URG 비트가

1일 때 사용

긴급 데이터 위치

실무에서는 거의 사용되지 않는다.

---

# Options

가변 길이

대표

- MSS
- Window Scale
- SACK

---

# Padding

Header를

4Byte 배수로 맞춘다.

---

# Header에서 가장 중요한 필드

★★★★★

- Source Port

- Destination Port

- Sequence Number

- ACK Number

- Window

- Checksum

교수님도

"이 여섯 개는 반드시 이해해야 한다."

라고 강조하셨다.

---

# Wireshark에서 보는 TCP Header

실무에서는

가장 먼저

- Port

- Sequence

- ACK

- Window

- Checksum

을 확인한다.

---

# 시험 핵심

✔ Source Port

✔ Destination Port

✔ Sequence Number

✔ ACK Number

✔ Window

✔ Checksum

---

# 암기법

```
Header

=

데이터를 안전하게 보내기 위한 설명서
```

---

# 면접 질문

TCP Header란?

Sequence Number란?

ACK Number란?

Window란?

Checksum은 왜 필요한가?

---

# 핵심 요약

TCP Header는

신뢰성 있는 통신을 위한

모든 제어 정보를 저장한다.

다음 장에서는

TCP Header의 Checksum을 계산하기 위해 사용하는

Pseudo Header를 학습한다.