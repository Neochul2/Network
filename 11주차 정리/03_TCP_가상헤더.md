# TCP/IP Advanced

# Chapter 03
# TCP 가상헤더(Pseudo Header)

---

# 학습 목표

이번 장에서는 TCP Checksum 계산에 사용되는 TCP 가상헤더(Pseudo Header)를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- TCP 가상헤더란 무엇인가?
- 왜 필요한가?
- 실제 전송되는가?
- Checksum은 어떻게 계산되는가?
- UDP 가상헤더와의 차이점

---

# TCP 가상헤더란?

TCP 가상헤더(Pseudo Header)는

TCP Header 앞에 **임시로 만들어지는 가상의 Header**이다.

중요한 점은

> **실제 네트워크로 전송되지 않는다.**

오직

**TCP Checksum을 계산하기 위해서만**

잠시 생성된다.

Checksum 계산이 끝나면

즉시 제거된다.

---

# 왜 필요한가?

TCP Checksum은

TCP Header와 Data만 검사하는 것이 아니다.

IP 계층 정보도 함께 검사해야 한다.

예를 들어

- 출발지 IP
- 목적지 IP

까지 함께 확인해야

잘못된 장비로 전달되는 오류를 검출할 수 있다.

그래서

IP Header의 일부 정보를

임시 Header로 만들어

Checksum 계산에 사용한다.

---

# 가상헤더 구조

가상헤더는 다음 필드로 구성된다.

```text
+-------------------------------+
| Source IP Address             |
+-------------------------------+
| Destination IP Address        |
+-------------------------------+
| Reserved                      |
+-------------------------------+
| Protocol (TCP=6)              |
+-------------------------------+
| TCP Length                    |
+-------------------------------+
```

---

# 각 필드 설명

## Source Address

송신자의 IPv4 주소

예

```
192.168.0.10
```

---

## Destination Address

수신자의 IPv4 주소

예

```
142.250.206.78
```

(Google)

---

## Reserved

항상

```
0
```

으로 채운다.

미래 사용을 위한 예약 필드이다.

---

## Protocol

상위 프로토콜 번호

TCP는

```
6
```

UDP는

```
17
```

이다.

---

## TCP Length

TCP Header

+

TCP Data

의 전체 길이이다.

IP Header는 포함하지 않는다.

---

# Checksum 계산 과정

TCP가 데이터를 보내기 전에

다음 순서로 계산한다.

```text
TCP Header

+

TCP Data

+

Pseudo Header

↓

Checksum 계산

↓

Checksum 값을 Header에 저장

↓

Pseudo Header 제거

↓

TCP Segment 전송
```

즉,

가상헤더는

계산이 끝나면 사라진다.

---

# 실제 전송되는가?

아니다.

가상헤더는

절대로 네트워크로 전송되지 않는다.

실제로 전송되는 것은

```text
TCP Header

+

Data
```

뿐이다.

---

# 교수님 수업 포인트

교수님께서는

다음 내용을 강조하셨다.

✔ 가상헤더는 임시 Header이다.

✔ Checksum 계산이 끝나면 버린다.

✔ Wireshark에서는 보이지 않는다.

✔ 학생들이 가장 많이 착각하는 부분이다.

---

# UDP와 비교

UDP도

Pseudo Header를 사용한다.

차이점은

Checksum 계산 방식은 비슷하지만

TCP는 신뢰성을 위해 더 적극적으로 활용한다.

---

# 왜 IP 주소까지 검사하는가?

예를 들어

출발지

```
192.168.0.10
```

↓

목적지

```
192.168.0.20
```

으로 보내야 하는데

중간에서

목적지 주소가 손상된다면

TCP Header만 검사해서는

오류를 발견하지 못할 수 있다.

그래서

IP 주소까지 포함하여

Checksum을 계산한다.

---

# Wireshark에서는?

Wireshark에는

Pseudo Header가

보이지 않는다.

왜냐하면

이미 제거된 상태로

TCP Segment가 전송되기 때문이다.

실제로 보이는 것은

- TCP Header
- Data
- Checksum

뿐이다.

---

# 시험 포인트

★★★★★

✔ Pseudo Header는 실제 전송되지 않는다.

✔ Checksum 계산에만 사용한다.

✔ Source IP와 Destination IP를 포함한다.

✔ Protocol 번호(TCP=6)를 포함한다.

✔ TCP Length를 포함한다.

---

# 암기법

Pseudo Header

=

> **"Checksum 계산용 임시 Header"**

---

# 면접 질문

### Q1. TCP Pseudo Header란 무엇인가?

### Q2. 왜 실제 전송하지 않는 Header를 만드는가?

### Q3. Pseudo Header에는 어떤 정보가 포함되는가?

### Q4. Wireshark에서 Pseudo Header를 볼 수 없는 이유는?

---

# 핵심 요약

- TCP Checksum 계산을 위해 Pseudo Header를 생성한다.
- Source IP, Destination IP, Protocol, TCP Length가 포함된다.
- 계산이 끝나면 삭제된다.
- 실제 네트워크에는 전송되지 않는다.
- Wireshark에서는 확인할 수 없다.

---

# 다음 장

다음 장에서는

TCP의 최대 세그먼트 크기인

**MSS(Maximum Segment Size)** 를 학습한다.

- MSS란?
- MTU와의 관계
- 왜 기본값이 536Byte인가?
- Fragmentation과 MSS의 관계