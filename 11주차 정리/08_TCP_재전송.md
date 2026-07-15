# TCP/IP Advanced

# Chapter 08
# TCP 재전송(Retransmission)

---

# 학습 목표

이번 장에서는 TCP가 데이터 손실을 어떻게 복구하는지 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- TCP 재전송이 필요한 이유
- Retransmission Queue
- Retransmission Timer
- Timeout
- 누적 ACK
- TCP 재전송 정책

---

# TCP는 왜 재전송이 필요한가?

인터넷에서는 모든 데이터가 반드시 정상적으로 도착하는 것은 아니다.

다음과 같은 문제가 발생할 수 있다.

- 패킷 손실(Packet Loss)
- 네트워크 혼잡(Congestion)
- 라우터 장애
- 전송 오류

TCP는 이러한 상황에서도 데이터를 안전하게 전달하기 위해 **재전송(Retransmission)** 기능을 제공한다.

---

# 재전송의 기본 원리

TCP는 데이터를 전송한 후

바로 잊어버리지 않는다.

먼저 복사본을 저장한다.

```text
데이터 전송

↓

복사본 저장

↓

ACK 대기

↓

ACK 도착

↓

복사본 삭제
```

ACK가 오지 않으면

복사본을 다시 전송한다.

---

# Retransmission Queue란?

재전송 큐(Retransmission Queue)는

아직 ACK를 받지 못한 TCP Segment를 저장하는 공간이다.

```text
송신

↓

재전송 큐 저장

↓

ACK 대기
```

ACK가 오기 전까지는

세그먼트가 큐에 남아 있다.

---

# 재전송 큐의 동작

예를 들어

```text
Segment A

↓

전송

↓

재전송 큐 저장
```

↓

ACK가 오면

```text
재전송 큐 삭제
```

↓

ACK가 오지 않으면

```text
다시 전송
```

---

# Retransmission Timer

세그먼트를 전송하면

TCP는 동시에 **재전송 타이머(Retransmission Timer)** 를 시작한다.

```text
Segment 전송

↓

Timer 시작

↓

ACK 도착?

↓

YES → Timer 종료

NO → Timeout
```

---

# Timeout

Timeout은

정해진 시간 안에 ACK가 도착하지 않은 상태이다.

예를 들어

```text
0초

Segment 전송

↓

1초

ACK 없음

↓

2초

ACK 없음

↓

Timeout 발생
```

↓

재전송

---

# ACK가 도착하면?

ACK가 도착하면

TCP는

```text
재전송 큐 삭제

↓

Timer 종료
```

를 수행한다.

즉,

재전송은 하지 않는다.

---

# ACK가 오지 않으면?

ACK가 오지 않으면

```text
Timeout

↓

재전송

↓

새 Timer 시작
```

이 과정을 반복한다.

---

# 누적 ACK(Cumulative ACK)

TCP는

개별 ACK를 보내지 않는다.

누적 ACK 방식을 사용한다.

예를 들어

```text
1~100

↓

101~200

↓

201~300
```

세 개의 Segment를 전송했다고 가정하자.

수신자는

모두 정상적으로 받으면

```text
ACK 301
```

하나만 보낸다.

의미는

```
1~300

모두 정상 수신

301부터 보내세요.
```

이다.

---

# 세그먼트가 승인되었다는 의미

TCP는

ACK 번호가

세그먼트의 마지막 Byte보다 크면

그 세그먼트는

완전히 승인된 것으로 판단한다.

예)

```text
Seq

1~100
```

↓

ACK

101

↓

완전 승인

---

# TCP 재전송 정책

교수님 PDF에서는

두 가지 정책을 설명한다.

---

## 정책 ①

Timeout이 발생한

세그먼트만

재전송

장점

- 대역폭 절약

단점

- 여러 개가 손실되면 느리다.

---

## 정책 ②

Timeout이 발생하면

ACK를 받지 못한

모든 세그먼트를

재전송

장점

- 복구 속도가 빠르다.

단점

- 불필요한 재전송 증가

- 대역폭 낭비

---

# 정책 비교

| 구분 | Timeout Segment만 | 모든 미승인 Segment |
|------|-------------------|---------------------|
| 속도 | 느림 | 빠름 |
| 대역폭 | 효율적 | 비효율적 |
| 손실 복구 | 느림 | 빠름 |

---

# 교수님 수업 포인트

교수님께서는

다음 내용을 반복해서 강조하셨다.

★★★★★

- ACK가 오면 큐에서 삭제
- ACK가 안 오면 재전송
- Timer는 ACK를 기다리는 시계
- TCP는 누적 ACK를 사용한다.

---

# Wireshark에서는?

재전송이 발생하면

다음과 같이 표시된다.

```text
[TCP Retransmission]
```

또는

```text
[Fast Retransmission]
```

ACK가 늦게 도착하면

같은 Sequence Number가

다시 전송되는 것을 확인할 수 있다.

---

# 실무 예시

100MB 파일을 다운로드하는 중

중간 Segment 하나가

손실되었다.

```text
1~100

↓

101~200

↓

201~300

(손실)

↓

301~400
```

ACK가 오지 않으면

TCP는

201~300 Segment를

다시 전송한다.

사용자는

거의 느끼지 못한다.

---

# 시험 포인트

★★★★★

✔ ACK가 오면 재전송 큐에서 삭제한다.

✔ ACK가 오지 않으면 Timeout 후 재전송한다.

✔ TCP는 누적 ACK를 사용한다.

✔ 재전송 정책은 두 가지가 있다.

---

# 암기법

```text
전송

↓

큐 저장

↓

Timer 시작

↓

ACK

↓

삭제

------------------

ACK 없음

↓

Timeout

↓

재전송
```

---

# 면접 질문

Q. Retransmission Queue란 무엇인가?

Q. Retransmission Timer의 역할은?

Q. Timeout이 발생하면 TCP는 어떻게 동작하는가?

Q. 누적 ACK란 무엇인가?

Q. TCP가 재전송을 사용하는 이유는?

---

# 핵심 요약

- TCP는 데이터를 전송한 후 재전송 큐에 저장한다.
- ACK가 오면 큐에서 제거한다.
- ACK가 오지 않으면 Timeout 후 재전송한다.
- TCP는 누적 ACK를 사용한다.
- 재전송 정책에는 Timeout Segment만 재전송하는 방법과 모든 미승인 Segment를 재전송하는 방법이 있다.

---

# 다음 장

Chapter 09에서는

TCP의 성능을 향상시키는 기능인

**Selective ACK(SACK)** 를 학습한다.

- SACK란?
- 누적 ACK와의 차이
- SACK의 장점
- 실제 동작 과정