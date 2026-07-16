# TCP/IP Advanced

# Chapter 08

# TCP 재전송 (Retransmission)

---

# 학습 목표

이번 장에서는 TCP가 데이터 손실을 어떻게 복구하는지 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- TCP 재전송이 필요한 이유
- Retransmission Queue
- Retransmission Timer
- Timeout(RTO)
- RTT와 RTO의 관계
- 누적 ACK(Cumulative ACK)
- Fast Retransmit
- TCP 재전송 정책

---

# 1. TCP는 왜 재전송이 필요한가?

인터넷에서는 모든 데이터가 항상 정상적으로 도착하지 않는다.

대표적인 원인

- Packet Loss
- 네트워크 혼잡(Congestion)
- 라우터 장애
- 전송 오류

TCP는 이러한 상황에서도 신뢰성 있는 전송을 위해 **재전송(Retransmission)**을 수행한다.

---

# 2. 재전송의 기본 원리

```text
데이터 전송
    │
    ▼
Retransmission Queue 저장
    │
    ▼
ACK 대기
    │
 ┌──┴──┐
 ▼     ▼
ACK   Timeout
 │       │
 ▼       ▼
승인된 데이터 제거
        │
        ▼
     재전송
```

ACK가 도착하면 **승인(ACK)된 Segment만 Queue에서 제거**하고, ACK가 오지 않으면 동일한 Segment를 다시 전송한다.

---

# 3. Retransmission Queue

Retransmission Queue는 **ACK를 받지 못한 TCP Segment를 관리하는 송신 측의 대기 영역**이다.

전송된 Segment는 ACK를 받을 때까지 이 영역에 보관되며, 필요하면 재전송된다.

```text
Part1
Part2
Part3
Part4
──────────────
ACK 대기
```

ACK가 도착하면 승인된 Segment부터 제거된다.

예)

```text
ACK = 201

↓

Part1 삭제
Part2 삭제

↓

Part3
Part4
```

TCP는 ACK를 받을 때까지 데이터를 보관하여 손실이 발생하면 동일한 Segment를 다시 전송할 수 있다.

---

# 4. Retransmission Timer

세그먼트를 전송하면 Retransmission Timer가 시작된다.

```text
Segment 전송

↓

Timer 시작

↓

ACK 도착?

YES → Timer 종료

NO → Timeout
          │
          ▼
      Segment 재전송
          │
          ▼
     새로운 Timer 시작
```

재전송 후에도 ACK를 기다리기 위해 새로운 Timer가 다시 시작된다.

---

# 5. RTT와 RTO

TCP는 Segment를 전송한 후 ACK가 도착할 때까지의 시간을 **RTT(Round Trip Time)** 라고 한다.

TCP는 매번 측정한 RTT를 단순 평균하지 않는다.

대신 **SRTT(Smoothed RTT)** 와 **RTTVAR(RTT Variance)** 를 계산하여 네트워크 상태를 지속적으로 반영한다.

이 값을 이용하여 재전송 시간인 **RTO(Retransmission Timeout)** 를 동적으로 결정한다.

```text
Segment 전송

↓

RTT 측정

↓

SRTT / RTTVAR 갱신

↓

RTO 계산

↓

Timeout 여부 판단
```

네트워크 지연이 증가하면 RTO도 증가하고,

네트워크 지연이 감소하면 RTO도 함께 감소한다.

> RFC 6298에서는 RTT의 단순 평균이 아니라 SRTT와 RTTVAR를 이용하여 RTO를 계산하도록 정의하고 있다.

---

# 6. Timeout

```text
0초   Segment 전송

↓

RTO Timer 시작

↓

ACK 없음

↓

RTO 만료

↓

Segment 재전송

↓

새로운 Timer 시작
```

ACK가 도착하지 않으면 RTO가 만료되고 동일한 Segment를 다시 전송한다.

---

# 7. 교수님 예제

```text
Part1
↓

Part2
↓

Part3 (손실)
↓

Part4
```

수신자는 Part3이 없으므로 이후 데이터가 도착해도 연속된 데이터가 완성되지 않는다.

따라서 계속 같은 ACK를 보낸다.

```text
ACK = Part3의 시작 Sequence Number
```

송신자는 Timeout이 발생하거나 Fast Retransmit 조건이 만족되면 **Part3만 다시 전송**한다.

---

# 8. 누적 ACK (Cumulative ACK)

TCP는 **누적 ACK(Cumulative ACK)** 방식을 사용한다.

```text
1 ~ 100
101 ~ 200
201 ~ 300
```

모두 정상적으로 수신하면

```text
ACK = 301
```

의미

> **1~300Byte까지 모두 정상적으로 받았으며, 다음은 301Byte부터 보내십시오.**

즉, 하나의 ACK가 여러 개의 Segment를 동시에 승인할 수 있다.

---

# 9. Fast Retransmit

현대 TCP는 Timeout을 기다리지 않는 경우가 있다.

수신자로부터 **동일한 Duplicate ACK를 3회 연속 수신하면** 송신자는 Timeout을 기다리지 않고 손실된 Segment를 즉시 재전송한다.

```text
Duplicate ACK

ACK100

↓

ACK100

↓

ACK100

↓

Fast Retransmit

↓

즉시 재전송
```

Fast Retransmit은 Timeout보다 빠르게 손실을 복구하여 전송 성능을 향상시킨다.

> 참고: 최초 ACK 이후 **Duplicate ACK 3개**를 추가로 수신하면 Fast Retransmit이 수행된다.

---

# 10. 재전송 정책

TCP 구현 방식(Reno, NewReno, CUBIC, BBR 등)에 따라 재전송 방식은 조금씩 다를 수 있다.

## 정책 ①

손실이 확인된 Segment부터 재전송

장점

- 불필요한 재전송 감소
- 대역폭 절약

단점

- 복구 속도가 느려질 수 있다.

---

## 정책 ②

필요에 따라 여러 미승인 Segment를 함께 재전송

장점

- 손실 복구가 빠르다.

단점

- 불필요한 재전송이 증가할 수 있다.

현대 TCP는 **Fast Retransmit**, **Fast Recovery**, **SACK(Selective Acknowledgment)** 등을 함께 사용하여 효율적으로 손실을 복구한다.

---

# 11. Wireshark

재전송이 발생하면 다음과 같은 정보를 확인할 수 있다.

```text
[TCP Retransmission]
```

또는

```text
[Fast Retransmission]
```

동일한 **Sequence Number를 가진 새로운 TCP Segment**가 다시 전송되는 것을 확인할 수 있다.

이를 통해 어떤 Segment가 손실되어 재전송되었는지 분석할 수 있다.

---

# 교수님 수업 포인트

★★★★★

- ACK가 오면 승인된 Segment를 Queue에서 제거한다.
- ACK가 오지 않으면 재전송한다.
- Timer는 ACK를 기다리는 시계이다.
- RTT를 기반으로 RTO를 계산한다.
- TCP는 누적 ACK(Cumulative ACK)를 사용한다.
- Duplicate ACK 3회 수신 시 Fast Retransmit을 수행한다.

---

# 시험 포인트

- Retransmission Queue의 역할
- Retransmission Timer와 Timeout
- RTT와 RTO의 관계
- 누적 ACK(Cumulative ACK)
- Fast Retransmit
- 재전송 정책

---

# 암기법

```text
전송
 ↓
Queue 저장
 ↓
Timer 시작
 ↓
ACK?
 ├─ YES → 승인된 데이터 제거
 └─ NO → Timeout
          ↓
       재전송
          ↓
     Timer 재시작
```

---

# 면접 질문

**Q1. Retransmission Queue란 무엇인가?**

**Q2. RTT와 RTO의 차이는 무엇인가?**

**Q3. Timeout이 발생하면 TCP는 어떻게 동작하는가?**

**Q4. Fast Retransmit이란 무엇인가?**

**Q5. TCP는 왜 재전송을 사용하는가?**

---

# 핵심 요약

- TCP는 ACK를 받을 때까지 Segment를 Retransmission Queue에 보관한다.
- ACK가 도착하면 승인된 Segment를 Queue에서 제거한다.
- ACK가 오지 않으면 RTO 만료 후 동일한 Segment를 재전송한다.
- RTT를 측정하고 **SRTT와 RTTVAR를 이용하여 RTO를 동적으로 계산한다.**
- TCP는 누적 ACK(Cumulative ACK)를 사용하므로 하나의 ACK가 여러 Segment를 동시에 승인할 수 있다.
- 현대 TCP는 Fast Retransmit, Fast Recovery, SACK 등을 이용하여 손실 복구 성능을 향상시킨다.
