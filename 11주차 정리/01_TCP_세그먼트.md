# TCP/IP Advanced

# Chapter 01
# TCP 세그먼트 (TCP Segment)

---

# 학습 목표

이 장에서는 TCP가 데이터를 전송할 때 사용하는 기본 단위인 TCP 세그먼트(TCP Segment)를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- TCP/IP Protocol Suite에서 TCP의 위치
- TCP Segment란 무엇인가
- TCP Segment가 생성되는 과정
- TCP Header와 Data의 역할
- TCP Header의 전체 구조
- TCP Segment와 IP Datagram의 관계

---

# 1. TCP/IP Protocol Suite 복습

TCP/IP는 하나의 프로토콜이 아니라 여러 프로토콜이 모여 있는 **프로토콜 집합(Protocol Suite)** 이다.

교수님은 수업에서

> "TCP/IP는 하나의 프로토콜이 아니라 여러 프로토콜의 집합이다."

라고 먼저 설명하셨다.

TCP는 이 프로토콜 집합 중 **전송 계층(Transport Layer)** 에서 동작하는 대표적인 프로토콜이다.

TCP/IP 계층은 다음과 같이 구성된다.

```text
Application Layer
        │
Transport Layer
        │
Internet Layer
        │
Network Interface Layer
```

이번 장부터는 **Transport Layer의 TCP**를 내부 구조까지 자세히 학습한다.

---

# 2. TCP Segment란?

TCP는 애플리케이션이 만든 데이터를 그대로 전송하지 않는다.

먼저 데이터를 전송하기 위해 필요한 제어 정보를 추가한다.

이 제어 정보가 들어 있는 부분을 **TCP Header**라고 하며,

Header가 추가된 하나의 전송 단위를 **TCP Segment**라고 한다.

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

TCP는 항상 **Segment 단위**로 데이터를 전송한다.

---

# 3. 왜 TCP Segment가 필요한가?

만약 데이터만 전송한다면 수신자는 다음과 같은 정보를 알 수 없다.

- 누가 보냈는가?
- 어느 프로그램으로 보내야 하는가?
- 몇 번째 데이터인가?
- 데이터가 손상되었는가?
- 다음에는 무엇을 보내야 하는가?

이를 해결하기 위해 TCP는 Header에 다양한 제어 정보를 기록한다.

Header 덕분에 TCP는

- 신뢰성
- 순서 보장
- 오류 검출
- 흐름 제어

를 수행할 수 있다.

---

# 4. TCP Segment의 구조

TCP Segment는 크게 두 부분으로 구성된다.

```text
+-------------------------+
|       TCP Header        |
+-------------------------+
|   Data(Payload)         |
+-------------------------+
```

### TCP Header

전송을 제어하기 위한 정보가 저장된다.

예)

- Port 번호
- Sequence Number
- ACK Number
- Window
- Checksum

### Data(Payload)

실제 사용자가 보내는 데이터가 저장된다.

예)

- HTML
- 이미지
- 동영상
- PDF
- 이메일
- 게임 데이터

TCP는 데이터의 내용을 이해하지 않는다.

그저 **Byte Stream**으로 전달할 뿐이다.

---

# 5. TCP Segment 생성 과정

웹 브라우저에서

```
https://www.google.com
```

에 접속한다고 가정해 보자.

### ① Application

브라우저가 HTTP 요청을 생성한다.

```text
GET / HTTP/1.1
```

↓

### ② TCP

TCP Header를 추가한다.

```text
TCP Header

+

HTTP Data
```

↓

### ③ TCP Segment 생성

```text
TCP Segment
```

↓

### ④ IP 계층

IP Header를 추가한다.

```text
IP Header

+

TCP Segment
```

↓

### ⑤ IP Datagram 생성

↓

### ⑥ Ethernet Frame 생성

↓

### ⑦ 네트워크 전송

이 과정을 **캡슐화(Encapsulation)** 라고 한다.

---

# 6. TCP Header 전체 구조

TCP Header에는 다양한 제어 정보가 포함된다.

```text
Source Port

Destination Port

Sequence Number

Acknowledgement Number

Data Offset

Reserved

Control Bits

Window

Checksum

Urgent Pointer

Options

Padding
```

이번 장에서는 전체 구조만 이해한다.

각 필드는 다음 장에서 하나씩 자세히 학습한다.

---

# 7. Header와 Data의 차이

많은 학생들이 처음에 혼동하는 부분이다.

### Header

데이터를 안전하게 보내기 위한 설명서

### Data

실제 사용자가 보내는 정보

예를 들어

사진을 보내면

```text
Header

↓

사진 데이터
```

가 되는 것이 아니라

```text
TCP Header

+

사진 데이터
```

가 하나의 Segment가 된다.

---

# 8. 교수님 수업 포인트

교수님께서는 수업에서 다음 내용을 강조하셨다.

- TCP/IP는 하나의 프로토콜이 아니라 Protocol Suite이다.
- 오늘부터는 TCP 내부 구조를 공부한다.
- TCP Segment는 Header와 Data로 구성된다.
- 이후 모든 내용은 TCP Header를 중심으로 설명된다.

즉,

**Header를 이해하면 TCP 전체를 이해할 수 있다.**

---

# 9. Wireshark에서는?

Wireshark에서 TCP 패킷을 선택하면 가장 먼저 보이는 것이 TCP Header이다.

주요 확인 항목은 다음과 같다.

- Source Port
- Destination Port
- Sequence Number
- ACK Number
- Window Size
- Checksum

실무에서도 TCP 문제를 분석할 때 가장 먼저 확인하는 영역이다.

---

# 시험 포인트

✅ TCP/IP는 하나의 프로토콜이 아니라 Protocol Suite이다.

✅ TCP는 Transport Layer에서 동작한다.

✅ TCP Segment = TCP Header + Data(Payload)

✅ TCP는 Segment 단위로 데이터를 전송한다.

✅ Header에는 제어 정보가 저장되고 Data에는 실제 데이터가 저장된다.

---

# 암기법

### TCP Segment

```text
TCP Segment

=

TCP Header

+

Data
```

### Header

> 데이터를 안전하게 보내기 위한 설명서

### Data

> 실제 사용자의 데이터

---

# 면접 질문

### Q1. TCP Segment란 무엇인가?

### Q2. TCP Header가 필요한 이유는 무엇인가?

### Q3. TCP Segment와 IP Datagram의 차이는 무엇인가?

### Q4. TCP Header와 Data의 차이를 설명하시오.

---

# 핵심 요약

- TCP는 전송 계층에서 사용하는 연결형 프로토콜이다.
- TCP는 데이터를 Segment 단위로 전송한다.
- TCP Segment는 Header와 Data로 구성된다.
- Header에는 데이터 전송을 위한 제어 정보가 저장된다.
- 이후 배우는 Sequence Number, ACK, Window, Checksum은 모두 TCP Header의 필드이다.

---

# 다음 장

다음 장에서는 TCP Header의 각 필드를 하나씩 자세히 학습한다.

- Source Port
- Destination Port
- Sequence Number
- ACK Number
- Data Offset
- Control Bits
- Window
- Checksum
- Urgent Pointer
- Options