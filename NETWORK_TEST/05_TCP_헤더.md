# TCP 헤더

> ⭐ TCP Header 기본 20Byte, 최대 60Byte / MSS = Payload만의 크기 (Header 미포함)

---

## TCP Segment 구조

```
+---------------------------+
|        TCP Header         |  ← 제어 정보 (최소 20Byte)
+---------------------------+
|     Payload(Data)         |  ← 실제 사용자 데이터
+---------------------------+
```

---

## TCP Header 구조

```
0               16              32
+---------------+---------------+
|   출발지 포트  |   목적지 포트  |
+---------------+---------------+
|           순서 번호            |
+-------------------------------+
|           승인 번호            |
+----+------+------+------------+
|HLEN| 예약 |제어비트|   윈도우   |
+---------------+---------------+
|    체크섬     |   긴급 포인터  |
+---------------+---------------+
|     옵션 (0~40Byte) + 패딩    |
+-------------------------------+
|              데이터            |
+-------------------------------+
```

---

## TCP Header 필드 상세

| 필드 | 크기 | 설명 |
|------|------|------|
| 출발지 포트 | 2Byte | 송신 프로그램 포트. 클라이언트→서버: 임시(Ephemeral) 포트 사용. 서버→클라이언트: Well-Known 포트 사용 |
| 목적지 포트 | 2Byte | 수신 프로그램 포트. 클라이언트→서버: Well-Known 포트. 서버→클라이언트: 임시 포트 |
| 순서 번호 | 4Byte | 이 Segment의 첫 번째 Byte 번호. SYN 메시지일 경우 ISN 값을 가짐 ★★★★★ |
| 승인 번호 | 4Byte | ACK 비트가 1일 때 유효. 수신자가 다음에 받고 싶은 Byte 번호 ★★★★★ |
| 데이터 오프셋(HLEN) | 4bit | TCP Header의 길이를 32bit Word 단위로 표현. 값×4 = Header Byte 수. 기본값 5 → 20Byte |
| 예약(Reserved) | 6bit | **나중에 사용하기 위해 예약된 필드. 현재는 항상 0으로 설정.** 향후 TCP 기능 확장 시 사용하기 위해 공간을 미리 남겨둔 것 |
| 제어 비트 | 6bit | URG/ACK/PSH/RST/SYN/FIN — TCP 연결 및 상태 제어 |
| 윈도우 | 2Byte | 세그먼트 송신자가 수신 가능한 버퍼 크기(Byte). Flow Control 핵심 필드 ★★★★★ |
| 체크섬 | 2Byte | TCP Header + Payload + Pseudo Header를 포함한 오류 검출값 |
| 긴급 포인터 | 2Byte | URG=1일 때만 사용. 긴급 데이터의 마지막 Byte 순서 번호. 실무에서는 거의 사용 안 함 |
| 옵션 | 가변 | MSS, Window Scale, SACK, Timestamp 등 추가 기능. 연결 수립 시 주로 사용 |
| 패딩 | 가변 | 옵션 필드 길이가 32bit 배수가 아닐 때 0을 채워 Header 길이를 맞춤 |

---

## 제어 비트 (Control Bits / Flags) 6가지

| 비트 | 역할 |
|------|------|
| URG | 긴급 데이터 표시 (Urgent Pointer 활성화) |
| ACK | 승인 번호 유효 (수신 확인) |
| PSH | 즉시 애플리케이션에 전달 |
| RST | 비정상 연결 종료 / 초기화 |
| SYN | 연결 수립 요청 (ISN 교환) |
| FIN | 정상 연결 종료 |

---

## TCP Pseudo Header (가상헤더)

체크섬 계산 시에만 임시로 생성. **실제로 전송되지 않는다.**

```
+-------------------------------+
| 출발지 IP 주소 (IP 헤더에서)   |  4Byte
+-------------------------------+
| 목적지 IP 주소 (IP 헤더에서)   |  4Byte
+-------------------------------+
| 예약(0) | 프로토콜(6) | TCP 길이|  4Byte
+-------------------------------+
```

| 필드 | 크기 | 설명 |
|------|------|------|
| 출발지 주소 | 4Byte | IP 헤더에서 추출 |
| 목적지 주소 | 4Byte | IP 헤더에서 추출 |
| 예약 | 1Byte | 항상 0 |
| 프로토콜 | 1Byte | TCP=6, UDP=17 |
| TCP 길이 | 2Byte | TCP 헤더+데이터 전체 길이 (IP 헤더 미포함) |

체크섬 계산 순서: Pseudo Header + TCP Header + Payload → 계산 후 Pseudo Header 제거 → TCP Segment 전송

---

## MSS (Maximum Segment Size)

**MSS = TCP Segment의 Payload(Data)가 가질 수 있는 최대 크기**
**TCP Header는 MSS에 포함되지 않는다!** ★★★★★

```
MSS = MTU - IP Header - TCP Header
```

### MSS가 필요한 이유

TCP는 가능한 많은 데이터를 한 번에 전송하고 싶지만, 세그먼트가 너무 크면 IP 계층에서 **Fragmentation(단편화)** 이 발생한다.

Fragmentation 문제점:
- 성능 저하
- 재조립 필요
- 패킷 손실 시 세그먼트 전체 재전송 필요

→ MSS로 적절한 크기를 제한하여 Fragmentation 없이 효율적으로 전송.

### MTU vs MSS

| 용어 | 설명 |
|------|------|
| MTU (Maximum Transmission Unit) | 네트워크에서 한 번에 전송 가능한 **IP Packet 전체**의 최대 크기 |
| MSS (Maximum Segment Size) | TCP Segment의 **Payload(Data)만**의 최대 크기 (Header 미포함) |

```
MTU = IP Header + TCP Header + Payload(Data)
MSS = MTU - IP Header - TCP Header
    = Payload(Data)만의 크기
```

### MSS 값 계산

| 환경 | MTU | IP Header | TCP Header | MSS |
|------|-----|-----------|------------|-----|
| 기본값 (RFC) | 576Byte | 20Byte | 20Byte | **536Byte** |
| Ethernet | 1500Byte | 20Byte | 20Byte | **1460Byte** |

### 기본 MSS 536Byte의 역사적 배경

초기 TCP/IP가 설계되던 시절, 인터넷의 모든 라우터와 호스트가 처리할 수 있는 **IPv4 최소 MTU는 576Byte**로 정의되었다 (RFC 791).

```
최소 MTU = 576Byte
IP Header = 20Byte
TCP Header = 20Byte
→ MSS = 576 - 20 - 20 = 536Byte
```

즉, 어떤 네트워크 경로를 거치더라도 Fragmentation 없이 전달될 수 있는 최소한의 안전한 Payload 크기가 **536Byte**였다.

> 현재 Ethernet 환경에서는 MTU=1500Byte이므로 MSS=1460Byte를 주로 사용하지만,
> **536Byte는 역사적 기본 MSS 값으로 시험에 자주 출제된다.**

### MSS 크기와 문제

```
MSS가 너무 작으면:
TCP Header(20) + IP Header(20) + Payload(40) → 헤더 비율 50% → 대역폭 낭비

MSS가 너무 크면:
MTU 초과 → Fragmentation 발생 → 성능 저하, 재전송 증가
```

- 3-Way Handshake 시 양쪽이 MSS 옵션 교환
- 일반적으로 양쪽 중 **작은 값**을 기준으로 통신

---

## TCP Segment 캡슐화 과정

```
Application Data
        ↓ TCP Header 추가
TCP Segment
        ↓ IP Header 추가
IP Datagram (Packet)
        ↓ Ethernet Header/Trailer 추가
Ethernet Frame
        ↓
물리 매체 (Bit) 전송
```

---

## 자주 틀리는 부분

❌ ACK Number는 마지막으로 받은 번호 → 다음에 받아야 할 번호  
❌ TCP Header는 항상 20Byte → Option 사용 시 최대 60Byte  
❌ MSS는 세그먼트 전체 크기 → Payload(Data)만의 크기  
❌ Pseudo Header도 전송된다 → Checksum 계산 후 삭제, 전송 안 됨
