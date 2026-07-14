# TCP/IP 네트워크

# PART 4. UDP(User Datagram Protocol)

> **학습 목표**
>
> - UDP가 왜 필요한지 이해한다.
> - UDP의 특징을 설명할 수 있다.
> - UDP Header의 구조를 이해한다.
> - UDP Checksum과 Virtual Header의 역할을 이해한다.

---

# 왜 UDP가 필요한가?

앞 장에서는

Port 번호를 이용하여

데이터를 올바른 프로그램으로 전달하는 방법을 배웠습니다.

하지만 아직 해결되지 않은 문제가 있습니다.

TCP는

신뢰성을 제공하기 위해

- 연결 생성
- ACK
- 재전송
- Timeout
- 흐름 제어

등의 기능을 수행합니다.

이러한 기능은

정확성을 높이는 대신

속도를 희생합니다.

모든 서비스가

이러한 기능을 필요로 하는 것은 아닙니다.

실시간 서비스는

정확성보다

빠른 전달이 더 중요합니다.

이러한 요구를 만족하기 위해

UDP(User Datagram Protocol)가 만들어졌습니다.

---

# UDP란?

UDP(User Datagram Protocol)는

전송계층(Transport Layer)의

비연결형(Connectionless)

프로토콜입니다.

UDP는

가능한 한 빠르게

데이터를 전달하는 것을 목표로 합니다.

TCP처럼

연결을 생성하거나

상대방의 응답을 기다리지 않습니다.

---

# UDP의 특징

UDP는 다음과 같은 특징을 가집니다.

- 비연결형(Connectionless)
- 신뢰성 제공 안 함
- ACK 사용 안 함
- 재전송 없음
- 순서 보장 없음
- 흐름 제어 없음
- 혼잡 제어 없음
- Header 크기 8Byte

즉

속도를 위해

복잡한 기능을 제거한

가장 단순한 전송계층 프로토콜입니다.

---

# UDP가 수행하는 기능

UDP는

Application에서 받은 데이터를

UDP Header와 함께

IP 계층으로 전달합니다.

```text
Application

↓

UDP

↓

IP

↓

Network
```

UDP의 역할은

여기까지입니다.

---

# UDP가 수행하지 않는 기능

UDP는

다음 기능을 제공하지 않습니다.

- 연결 생성
- 연결 종료
- ACK
- 재전송
- 순서 보장
- Flow Control
- Congestion Control

따라서

데이터가 손실되어도

UDP는

다시 보내지 않습니다.

---

# UDP Header

UDP Header는

총 8Byte입니다.

구조는 다음과 같습니다.

```text
0                15 16               31

+----------------+----------------+

| Source Port    | Destination Port|

+----------------+----------------+

| Length         | Checksum        |

+----------------+----------------+
```

TCP Header보다

훨씬 단순합니다.

---

# Source Port

송신 프로그램의 Port 번호입니다.

예)

```text
52341
```

---

# Destination Port

수신 프로그램의 Port 번호입니다.

예)

```text
80

53

443
```

---

# Length

UDP Header와

Payload를 포함한

전체 길이를 나타냅니다.

단위는 Byte입니다.

---

# Checksum

Checksum은

전송 중

데이터 오류를 검사하기 위한 필드입니다.

Checksum이 다르면

패킷이 손상된 것으로 판단합니다.

IPv4에서는

선택적으로 사용할 수 있습니다.

IPv6에서는

반드시 사용해야 합니다.

---

# UDP Virtual Header

UDP는

Checksum 계산 시

Virtual Header를 사용합니다.

Virtual Header에는

- Source IP
- Destination IP
- Protocol 번호
- UDP Length

정보가 포함됩니다.

중요한 점은

Virtual Header는

Checksum 계산에만 사용되며

실제로 전송되지는 않습니다.

---

# UDP의 동작 과정

```text
Application

↓

UDP Header 추가

↓

IP

↓

Network

↓

수신

↓

Application
```

UDP는

데이터를 전송한 후

응답을 기다리지 않습니다.

---

# UDP를 사용하는 서비스

대표적인 UDP 서비스

- DNS
- DHCP
- VoIP
- IPTV
- 온라인 게임
- 실시간 스트리밍
- SNMP
- NTP

이 서비스들은

정확성보다

빠른 응답이 중요합니다.

---

# TCP와 UDP 비교

|항목|TCP|UDP|
|---|---|---|
|연결|연결형|비연결형|
|신뢰성|O|X|
|ACK|O|X|
|재전송|O|X|
|순서 보장|O|X|
|Header 크기|20~60Byte|8Byte|
|속도|느림|빠름|

---

# 실제 예시

DNS 서버에 질의하는 경우

```text
PC

↓

UDP 53

↓

DNS Server

↓

응답
```

응답이 오지 않으면

UDP가 아니라

응용프로그램이

다시 요청합니다.

---

# 왜 다음 장이 필요한가?

UDP는

빠른 전송을 제공합니다.

하지만

데이터가 손실되거나

순서가 바뀌어도

복구하지 않습니다.

그렇다면

신뢰성 있는 통신은

어떻게 구현할까요?

이를 해결하는 프로토콜이

TCP입니다.

다음 장에서는

TCP의 구조와

제공 기능을 학습합니다.

---

# 시험 포인트

- UDP는 비연결형 프로토콜이다.
- UDP Header는 8Byte이다.
- UDP는 ACK를 사용하지 않는다.
- UDP는 재전송하지 않는다.
- Virtual Header는 Checksum 계산에만 사용된다.
- Virtual Header는 실제 전송되지 않는다.

---

# 자주 틀리는 부분

❌ UDP는 오류가 발생하면 자동으로 재전송한다.

→ 아니다.

재전송은 TCP의 기능이다.

---

❌ Virtual Header도 네트워크로 전송된다.

→ 아니다.

Checksum 계산에만 사용된다.

---

# 핵심 요약

- UDP는 빠른 데이터 전송을 위한 비연결형 프로토콜이다.
- UDP는 연결 생성, ACK, 재전송을 수행하지 않는다.
- UDP Header의 크기는 8Byte이다.
- Checksum은 오류 검출을 담당한다.
- Virtual Header는 Checksum 계산용이며 실제로 전송되지 않는다.

---

# 다음 장

**PART 5. TCP(Transmission Control Protocol)**