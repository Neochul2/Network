# TCP/IP Advanced

# Chapter 04
# MSS (Maximum Segment Size)

---

# 학습 목표

이번 장에서는 TCP에서 사용하는 MSS(Maximum Segment Size)를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- MSS란 무엇인가?
- MTU와 MSS의 차이
- MSS가 필요한 이유
- 기본 MSS가 536Byte인 이유
- Fragmentation과 MSS의 관계

---

# MSS란?

MSS(Maximum Segment Size)는

TCP 세그먼트에서 **Data(Payload)가 가질 수 있는 최대 크기**를 의미한다.

즉,

> **TCP Header는 MSS에 포함되지 않는다.**

많은 학생들이

"세그먼트 전체 크기"

라고 착각하는데

그것은 틀린 표현이다.

MSS는

오직

**Data 크기만 의미한다.**

---

# 교수님 강조

수업에서 가장 많이 강조한 내용이다.

> "MSS는 Header가 아니다."

> "Data만 계산한다."

시험에서도

이 부분이 자주 출제된다.

★★★★★

---

# MSS와 TCP Segment

예를 들어

```text
TCP Header

20Byte

+

Data

1460Byte
```

이면

전체 Segment 크기는

```text
1480Byte
```

이다.

하지만

MSS는

```text
1460Byte
```

이다.

Header는

포함하지 않는다.

---

# 왜 MSS가 필요한가?

TCP는

가능하면

한 번에

많은 데이터를 보내고 싶다.

하지만

너무 크게 보내면

IP 계층에서

Fragmentation(단편화)

이 발생한다.

단편화가 발생하면

성능이 크게 떨어진다.

그래서

TCP는

MTU보다 작은

적절한 크기의 Segment를 만든다.

이 기준이

MSS이다.

---

# MSS와 MTU의 차이

가장 많이 헷갈리는 개념이다.

## MTU

Maximum Transmission Unit

↓

한 번에 보낼 수 있는

최대 Frame 크기

---

## MSS

Maximum Segment Size

↓

TCP Data의

최대 크기

---

# 관계

```text
MTU

=

IP Header

+

TCP Header

+

TCP Data
```

즉

```text
MSS

=

MTU

-

IP Header

-

TCP Header
```

---

# Ethernet에서의 MSS

Ethernet MTU는

```text
1500Byte
```

이다.

IPv4 Header

```
20Byte
```

TCP Header

```
20Byte
```

따라서

```text
1500

-

20

-

20

=

1460Byte
```

즉

Ethernet에서는

보통

```
MSS

1460Byte
```

를 사용한다.

---

# 기본 MSS 536Byte

교수님께서

시험에서 자주 나온다고

강조한 내용이다.

초기 TCP에서는

최소 MTU를

576Byte로

가정하였다.

따라서

```text
576

-

20(IP)

-

20(TCP)

=

536Byte
```

즉

기본 MSS는

```
536Byte
```

이다.

★★★★★

시험 출제 빈도가 매우 높다.

---

# MSS가 너무 작으면?

예를 들어

MSS가

```
40Byte
```

라면

Header

```
40Byte
```

Data

```
40Byte
```

총

```
80Byte
```

중

실제 데이터는

절반뿐이다.

Header 비율이

너무 커진다.

따라서

대역폭을

비효율적으로 사용하게 된다.

---

# MSS가 너무 크면?

이번에는

MSS를

3000Byte로

설정했다고

가정하자.

Ethernet MTU는

1500Byte

이므로

IP 계층에서

Fragmentation이

발생한다.

문제점

- 성능 저하

- 재조립 필요

- 패킷 손실 증가

- 재전송 증가

따라서

MSS는

MTU보다

작게

설정해야 한다.

---

# Fragmentation과 MSS

```text
MSS 작다

↓

Fragmentation 없음

↓

효율 좋음
```

반대로

```text
MSS 크다

↓

Fragmentation 발생

↓

성능 저하
```

---

# 교수님 수업 포인트

교수님께서는

다음 내용을

여러 번 강조하셨다.

✔ MSS는 Data만 의미한다.

✔ Header는 포함하지 않는다.

✔ 기본 MSS는

536Byte

이다.

✔ MTU와 헷갈리지 말 것.

---

# 실무에서는?

Wireshark에서는

TCP SYN

패킷 안의

Options에서

MSS 값을 확인할 수 있다.

예)

```
MSS = 1460
```

클라이언트와 서버는

3-Way Handshake 과정에서

서로 MSS를 교환한다.

작은 값을 기준으로

통신한다.

---

# 시험 포인트

★★★★★

✔ MSS는 Data 크기이다.

✔ Header는 포함하지 않는다.

✔ MSS = MTU - IP Header - TCP Header

✔ 기본 MSS = 536Byte

✔ Ethernet MSS = 1460Byte

---

# 암기법

```
MSS

=

Data

만!
```

Header는

절대 포함하지 않는다.

---

# 면접 질문

Q.

MSS란 무엇인가?

Q.

MTU와 MSS의 차이는?

Q.

기본 MSS가

536Byte인 이유는?

Q.

Ethernet에서

MSS가

1460Byte인 이유는?

---

# 핵심 요약

- MSS는 TCP Data의 최대 크기이다.
- Header는 MSS에 포함되지 않는다.
- MSS는 MTU보다 작아야 한다.
- Ethernet에서는 일반적으로 1460Byte를 사용한다.
- 기본 MSS는 536Byte이다.
- MSS는 Fragmentation을 방지하기 위해 존재한다.

---

# 다음 장

다음 장에서는

TCP의 핵심 기술인

Sliding Window를

심화 과정으로 학습한다.

- 송신 카테고리
- 수신 카테고리
- Window 이동
- ACK 관계
- Buffer 관계