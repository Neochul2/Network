# TCP/IP Advanced

# Chapter 09
# SACK (Selective Acknowledgement)

---

# 학습 목표

이번 장에서는 TCP의 선택적 승인(Selective Acknowledgement, SACK)을 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- SACK란 무엇인가?
- 왜 SACK가 필요한가?
- 누적 ACK와의 차이
- SACK의 동작 과정
- SACK의 장점

---

# SACK란?

SACK(Selective Acknowledgement)은

TCP의 선택적 승인 기능이다.

기본 TCP는

**누적 ACK(Cumulative ACK)**

만 사용한다.

하지만

누적 ACK에는 단점이 있다.

이 단점을 해결하기 위해

SACK가 만들어졌다.

---

# 누적 ACK의 문제점

예를 들어

다음과 같이

5개의 Segment를 보냈다고 하자.

```text
Segment1

↓

Segment2

↓

Segment3

↓

Segment4

↓

Segment5
```

그런데

Segment3만

손실되었다.

```text
Segment1

O

Segment2

O

Segment3

X

Segment4

O

Segment5

O
```

수신자는

4와 5를

받았지만

3이 없기 때문에

ACK를

계속

3번 위치로

보낸다.

송신자는

4와 5를

이미 받았는지

알 수 없다.

---

# 결과

송신자는

다시

3

4

5

를

모두

재전송한다.

이미 받은

4와 5까지

다시 보내므로

대역폭이

낭비된다.

---

# SACK의 등장

SACK는

이미 받은

Segment를

송신자에게

알려준다.

즉,

수신자는

다음과 같이

응답한다.

```text
ACK

3

↓

하지만

4

5

는

이미 받았습니다.
```

송신자는

3만

다시

전송하면 된다.

---

# 동작 과정

### Step 1

송신

```text
1

2

3

4

5
```

↓

### Step 2

수신

```text
1

2

O

3

X

4

O

5

O
```

↓

### Step 3

수신자는

ACK

3

+

SACK

4

5

를

보낸다.

↓

### Step 4

송신자는

3만

재전송한다.

---

# 누적 ACK와 비교

## 누적 ACK

```text
1

2

3

4

5
```

↓

3 손실

↓

재전송

```text
3

4

5
```

---

## SACK

```text
1

2

3

4

5
```

↓

3 손실

↓

재전송

```text
3
```

만

보낸다.

---

# 장점

SACK를 사용하면

- 불필요한 재전송 감소
- 대역폭 절약
- 전송 속도 향상
- 성능 향상

효과가 있다.

특히

대용량 파일 전송에서

효과가 크다.

---

# SACK는 어디에 저장되는가?

SACK는

TCP Header의

Options 필드를

사용한다.

즉

기본 Header에는

없다.

Option으로

추가된다.

---

# 교수님 수업 포인트

교수님께서는

다음 내용을 강조하셨다.

★★★★★

기본 TCP

↓

누적 ACK

SACK

↓

이미 받은 데이터도

알려준다.

즉

재전송을

줄인다.

---

# Wireshark에서는?

SACK가 사용되면

TCP Options에서

다음과 같이

확인할 수 있다.

```text
SACK Permitted
```

또는

```text
SACK Blocks
```

가 표시된다.

이를 통해

수신자가

어떤 구간을

이미 받았는지

확인할 수 있다.

---

# 실무 예시

1GB 파일을

다운로드하는 중

중간 Segment

하나가

손실되었다.

기본 TCP라면

뒤의

Segment까지

다시

보낼 수도 있다.

하지만

SACK를 사용하면

손실된

Segment만

재전송한다.

따라서

다운로드 속도가

더 빨라진다.

---

# 누적 ACK와 SACK 비교

| 구분 | 누적 ACK | SACK |
|------|----------|------|
| ACK 방식 | 순차 승인 | 선택적 승인 |
| 재전송 | 여러 Segment | 손실된 Segment만 |
| 대역폭 | 비효율 | 효율적 |
| 성능 | 보통 | 우수 |

---

# 시험 포인트

★★★★★

✔ SACK는 Selective ACK이다.

✔ 누적 ACK의 단점을 보완한다.

✔ 이미 받은 Segment를 알려준다.

✔ 손실된 Segment만 재전송한다.

✔ TCP Options 필드를 사용한다.

---

# 암기법

```text
ACK

↓

전체 승인

------------------

SACK

↓

선택 승인
```

즉

**"필요한 것만 다시 보낸다."**

---

# 면접 질문

Q. SACK란 무엇인가?

Q. 누적 ACK와 SACK의 차이는?

Q. SACK의 장점은?

Q. SACK는 TCP Header의 어느 필드를 사용하는가?

---

# 핵심 요약

- SACK는 Selective Acknowledgement의 약자이다.
- 누적 ACK의 단점을 보완하기 위해 만들어졌다.
- 수신자는 이미 받은 Segment를 송신자에게 알려준다.
- 송신자는 손실된 Segment만 재전송한다.
- SACK는 TCP Header의 Options 필드를 사용한다.
- 대용량 데이터 전송에서 성능 향상 효과가 크다.

---

# 다음 장

Chapter 10에서는

TCP의 흐름 제어(Flow Control)를 학습한다.

- Window Size 조절
- Receiver Buffer
- Zero Window
- Window Advertisement
- Flow Control의 실제 동작