# TCP/IP Advanced
# Chapter 01
# TCP 세그먼트(TCP Segment)

## 학습 목표

이 장에서는 **TCP가 데이터를 전송할 때 사용하는 기본 단위인 TCP 세그먼트(TCP Segment)**를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- TCP/IP Protocol Suite에서 TCP의 위치
- TCP Segment의 개념
- TCP Segment가 생성되는 과정
- TCP Header와 Payload(Data)의 역할
- TCP Header의 전체 구조
- TCP Segment와 IP Datagram(Packet)의 관계

---

# 1. TCP/IP Protocol Suite 복습

TCP/IP는 하나의 프로토콜이 아니라 여러 프로토콜이 모여 구성된 **프로토콜 집합(Protocol Suite)**이다.

교수님께서는 수업에서 먼저 다음 내용을 강조하셨다.

> "TCP/IP는 하나의 프로토콜이 아니라 여러 프로토콜이 함께 동작하는 프로토콜 집합이다."

TCP/IP는 일반적으로 다음과 같은 **4계층 모델(TCP/IP Model)**로 표현한다.

```text
Application Layer
        │
Transport Layer
        │
Internet Layer
        │
Network Interface Layer
```

각 계층은 자신의 역할만 수행하며, 서로 협력하여 데이터를 전송한다.

이번 장부터는 **Transport Layer의 대표 프로토콜인 TCP**를 중심으로 내부 구조를 학습한다.

---

# 2. TCP Segment란?

애플리케이션은 웹 페이지, 이메일, 이미지, 동영상과 같은 다양한 데이터를 생성한다.

TCP는 이러한 데이터를 그대로 전송하지 않는다.

먼저 데이터를 **안전하고 신뢰성 있게 전송하기 위해 필요한 다양한 제어 정보(Control Information)**를 추가한다.

이 제어 정보가 저장되는 부분을 **TCP Header**라고 한다.

TCP Header가 추가된 하나의 전송 단위를 **TCP Segment**라고 한다.

즉,

```text
Application Data

↓

TCP Header 추가

↓

TCP Segment 생성

↓

IP 계층으로 전달
```

TCP는 데이터를 **Segment라는 전송 단위**로 나누어 전송한다.

---

# 3. 왜 TCP Segment가 필요한가?

만약 데이터만 전송한다면 수신자는 다음과 같은 정보를 알 수 없다.

- 누가 보냈는가?
- 어느 프로그램으로 전달해야 하는가?
- 데이터의 순서는 무엇인가?
- 데이터가 손상되었는가?
- 다음에는 어떤 데이터를 받아야 하는가?

이를 해결하기 위해 TCP는 Header에 다양한 제어 정보를 저장한다.

TCP Header 덕분에 다음과 같은 기능을 수행할 수 있다.

- 신뢰성(Reliability)
- 순서 보장(Ordered Delivery)
- 오류 검출(Error Detection)
- 흐름 제어(Flow Control)
- 재전송(Retransmission)

즉,

> **TCP Header는 데이터를 안전하고 정확하게 전달하기 위한 다양한 제어 정보를 저장하는 영역이다.**

---

# 4. TCP Segment의 구조

TCP Segment는 크게 두 부분으로 구성된다.

```text
+---------------------------+
|        TCP Header         |
+---------------------------+
|     Payload(Data)         |
+---------------------------+
```

## ① TCP Header

TCP Header에는 데이터 전송을 위한 제어 정보가 저장된다.

대표적인 필드는 다음과 같다.

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- Window
- Checksum
- Control Bits
- Options

이러한 정보는 데이터의 신뢰성 있는 전송을 위해 사용된다.

---

## ② Payload(Data)

Payload는 실제 사용자가 전송하는 데이터이다.

예를 들어

- HTML 문서
- 이미지(JPG, PNG)
- 동영상(MP4)
- PDF 파일
- 이메일
- 게임 데이터

등이 모두 Payload가 된다.

TCP는 Payload의 내용을 해석하지 않는다.

TCP는 Payload를 단순한 **Byte Stream**으로 취급하여 전송할 뿐이다.

---

# 5. TCP Segment 생성 과정

웹 브라우저에서

```text
https://www.google.com
```

에 접속한다고 가정해 보자.

데이터는 다음과 같은 순서로 캡슐화된다.

### ① Application Layer

브라우저가 HTTP 요청을 생성한다.

```http
GET / HTTP/1.1
```

HTTPS의 경우에는 이후 TLS 계층에서 암호화가 수행된다.

↓

### ② Transport Layer(TCP)

TCP가 Header를 추가한다.

```text
TCP Header

+

Application Data
```

↓

### ③ TCP Segment 생성

```text
TCP Segment
```

↓

### ④ Internet Layer(IP)

IP가 Header를 추가한다.

```text
IP Header

+

TCP Segment
```

↓

### ⑤ IP Datagram(Packet) 생성

↓

### ⑥ Network Interface Layer

Ethernet Header와 Trailer가 추가된다.

↓

### ⑦ Ethernet Frame 생성

↓

### ⑧ 물리 매체(Bit)로 전송

이와 같이 상위 계층의 데이터 앞에 Header를 추가하면서 내려가는 과정을 **캡슐화(Encapsulation)**라고 한다.

반대로 수신 측에서는 Header를 하나씩 제거하는 **역캡슐화(Decapsulation)**가 수행된다.

---

# 6. TCP Header 전체 구조

TCP Header는 **최소 20Byte, 최대 60Byte**까지 사용할 수 있다.

기본 구조는 다음과 같다.

```text
  0                   15 16                  31
 +----------------------+----------------------+
 |     Source Port      |   Destination Port   |
 +----------------------+----------------------+
 |               Sequence Number               |
 +---------------------------------------------+
 |            Acknowledgement Number           |
 +----+------+----------+----------------------+
 |DOFF|Reserved| Flags  |       Window         |
 +----------------------+----------------------+
 |      Checksum        |    Urgent Pointer    |
 +----------------------+----------------------+
 |    Options (0~40Byte) + Padding             |
 +---------------------------------------------+
 |              Payload(Data)                  |
 +---------------------------------------------+
```

TCP Header에는 다음과 같은 필드가 포함된다.

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- Data Offset
- Reserved
- Control Bits(Flags)
- Window
- Checksum
- Urgent Pointer
- Options
- Padding

이번 장에서는 전체 구조만 이해하고,

각 필드는 다음 장부터 하나씩 자세히 학습한다.

---

# 7. Header와 Payload(Data)의 차이

처음 TCP를 공부하는 학생들이 가장 많이 혼동하는 부분이다.

## Header

데이터를 전송하기 위한 제어 정보를 저장하는 영역

예)

- Port 번호
- Sequence Number
- ACK Number
- Window
- Checksum

---

## Payload(Data)

실제 사용자가 보내는 데이터

예)

사진을 전송한다면

```text
TCP Header

+

사진 데이터
```

가 하나의 TCP Segment가 된다.

즉,

- Header는 데이터를 설명하는 영역
- Payload는 실제 데이터가 저장되는 영역

---

# 8. 교수님 수업 포인트

교수님께서는 다음 내용을 특히 강조하셨다.

- TCP/IP는 하나의 프로토콜이 아니라 Protocol Suite이다.
- TCP는 Transport Layer에서 동작한다.
- TCP Segment는 Header와 Payload(Data)로 구성된다.
- 이후 학습하는 모든 내용은 TCP Header를 중심으로 진행된다.

즉,

> **TCP Header를 이해하면 TCP의 핵심 동작 원리를 이해할 수 있다.**

---

# 9. Wireshark에서는?

Wireshark에서는 TCP 통신을 분석할 때 TCP Segment의 Header 정보를 확인할 수 있다.

주요 확인 항목은 다음과 같다.

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- Window Size
- Checksum
- Flags(SYN, ACK, FIN, RST 등)

실무에서도 TCP 문제를 분석할 때 가장 먼저 확인하는 영역이다.

---

# 시험 포인트

- ✅ TCP/IP는 하나의 프로토콜이 아니라 Protocol Suite이다.
- ✅ TCP는 Transport Layer에서 동작한다.
- ✅ TCP는 데이터를 Segment 단위로 전송한다.
- ✅ TCP Segment = TCP Header + Payload(Data)
- ✅ Header에는 제어 정보가 저장되고 Payload에는 실제 데이터가 저장된다.

---

# 암기법

```text
TCP Segment

=

TCP Header

+

Payload(Data)
```

**Header**

- 데이터를 전송하기 위한 제어 정보

**Payload**

- 실제 사용자의 데이터

---

# 면접 질문

**Q1. TCP Segment란 무엇인가?**

**Q2. TCP Header가 필요한 이유는 무엇인가?**

**Q3. TCP Segment와 IP Datagram(Packet)의 차이를 설명하시오.**

**Q4. TCP Header와 Payload(Data)의 차이를 설명하시오.**

**Q5. TCP Segment가 생성되는 과정을 설명하시오.**

---

# 핵심 요약

- TCP는 전송 계층(Transport Layer)에서 사용하는 연결 지향 프로토콜이다.
- TCP는 데이터를 Segment 단위로 전송한다.
- TCP Segment는 Header와 Payload(Data)로 구성된다.
- Header에는 데이터 전송을 위한 제어 정보가 저장된다.
- Payload에는 실제 사용자 데이터가 저장된다.
- TCP Segment는 IP 계층으로 전달되어 IP Datagram(Packet)이 되며, 이후 Ethernet Frame으로 캡슐화되어 전송된다.
