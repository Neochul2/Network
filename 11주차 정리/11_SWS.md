# TCP/IP Advanced

# Chapter 11
# Silly Window Syndrome (SWS)

---

# 학습 목표

이번 장에서는 TCP에서 발생할 수 있는 성능 저하 현상인
Silly Window Syndrome(SWS)을 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- SWS란 무엇인가?
- 왜 발생하는가?
- 어떤 문제가 생기는가?
- 송신 측 해결 방법
- 수신 측 해결 방법

---

# SWS란?

SWS(Silly Window Syndrome)는

TCP에서 매우 작은 크기의 데이터를
계속 전송하여

네트워크 효율이 크게 떨어지는 현상이다.

즉,

> **"보낼 수 있다고 해서 너무 작은 데이터를 계속 보내는 현상"**

이다.

---

# 왜 문제가 되는가?

TCP Header는

기본적으로

20Byte이다.

만약

```
1Byte
```

의 데이터만 보낸다면

```text
TCP Header

20Byte

+

Data

1Byte
```

총

21Byte를 전송하면서

실제 데이터는

1Byte뿐이다.

전송 효율이

매우 나빠진다.

---

# 발생 원인

SWS는

송신자와 수신자

모두에서 발생할 수 있다.

대표적인 원인은

- 작은 Window
- 작은 데이터
- 자주 발생하는 ACK
- Buffer를 조금씩만 비우는 애플리케이션

이다.

---

# 송신 측에서 발생하는 경우

예를 들어

Window가

```
10Byte
```

라고 가정하자.

송신자는

Window가 열릴 때마다

10Byte씩

계속 전송한다.

```text
10Byte

↓

ACK

↓

10Byte

↓

ACK

↓

10Byte
```

데이터는 전달되지만

패킷 수가 너무 많아진다.

---

# 수신 측에서 발생하는 경우

수신 애플리케이션이

버퍼를

조금씩만 읽는 경우를 생각해 보자.

예를 들어

```text
1000Byte Buffer
```

에서

매번

```
5Byte
```

만 비운다면

TCP는

```text
Window = 5
```

를 계속 광고(Advertise)하게 된다.

송신자는

5Byte씩만

계속 전송하게 된다.

결과적으로

SWS가 발생한다.

---

# 문제점

SWS가 발생하면

- 패킷 수 증가
- Header 오버헤드 증가
- CPU 사용량 증가
- 네트워크 대역폭 낭비
- 전체 전송 속도 저하

가 발생한다.

---

# 해결 방법 ①

## Clark Solution

Clark Solution은

수신 측에서 사용하는 방법이다.

수신자는

버퍼가 조금 비었다고

바로 Window를 열지 않는다.

충분한 공간이 확보될 때까지

기다렸다가

한 번에

큰 Window를 광고한다.

예)

```text
Window = 5

×

전송 안 함

↓

Window = 500

↓

광고
```

---

# 해결 방법 ②

## Nagle Algorithm

Nagle Algorithm은

송신 측에서 사용하는 방법이다.

송신자는

작은 데이터를

즉시 보내지 않는다.

여러 개를

모아서

한 번에 전송한다.

예)

```text
1Byte

+

2Byte

+

3Byte

+

4Byte

↓

10Byte

한 번 전송
```

---

# Clark Solution과 Nagle Algorithm 비교

| 구분 | Clark Solution | Nagle Algorithm |
|------|----------------|-----------------|
| 적용 위치 | 수신 측 | 송신 측 |
| 목적 | 작은 Window 광고 방지 | 작은 데이터 전송 방지 |
| 효과 | Window 증가 후 광고 | 데이터 모아서 전송 |

---

# 교수님 수업 포인트

교수님께서는

다음 내용을 강조하셨다.

★★★★★

SWS는

작은 데이터를

계속 보내는 문제이다.

★★★★★

Clark

↓

수신 측 해결

★★★★★

Nagle

↓

송신 측 해결

시험에서 자주 출제되는 부분이다.

---

# Wireshark에서는?

SWS가 발생하면

매우 작은 길이의

TCP Segment가

연속해서 나타난다.

예)

```text
Length = 1

Length = 2

Length = 3

Length = 1

Length = 4
```

이처럼

Payload가

매우 작은 Segment가

반복적으로 전송된다.

---

# 실무 예시

원격 터미널(Telnet, SSH)에서는

사용자가

키보드를

한 글자씩 입력한다.

매 글자마다

TCP Segment를 보내면

SWS가 발생할 수 있다.

그래서

운영체제는

Nagle Algorithm을 사용하여

여러 글자를

묶어서 전송한다.

---

# 시험 포인트

★★★★★

✔ SWS는 작은 데이터를 반복적으로 전송하여 성능이 저하되는 현상이다.

✔ Header 오버헤드가 증가한다.

✔ Clark Solution은 수신 측 해결 방법이다.

✔ Nagle Algorithm은 송신 측 해결 방법이다.

---

# 암기법

```text
SWS

↓

작은 데이터

↓

성능 저하

-------------------

Clark

↓

수신 측

-------------------

Nagle

↓

송신 측
```

---

# 면접 질문

Q. Silly Window Syndrome이란 무엇인가?

Q. 왜 발생하는가?

Q. Clark Solution의 역할은?

Q. Nagle Algorithm은 무엇인가?

Q. 두 방법의 차이는 무엇인가?

---

# 핵심 요약

- SWS는 작은 데이터를 반복적으로 전송하여 발생하는 성능 저하 현상이다.
- 작은 Window와 작은 Payload가 주요 원인이다.
- Clark Solution은 수신 측에서 작은 Window 광고를 방지한다.
- Nagle Algorithm은 송신 측에서 작은 데이터를 모아 전송한다.
- 두 기법 모두 네트워크 효율을 향상시키기 위해 사용된다.

---

# 다음 장

Chapter 12에서는

DNS(Domain Name System)를 학습한다.

- DNS의 필요성
- Domain Name
- Name Space
- Name Resolution
- DNS 동작 과정
- Google Public DNS(8.8.8.8)
- 실제 웹 접속 예제