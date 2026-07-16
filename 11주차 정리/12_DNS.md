# TCP/IP Advanced

# Chapter 12

# DNS (Domain Name System)

------------------------------------------------------------------------

# 학습 목표

이번 장에서는 DNS(Domain Name System)의 개념과 설계 철학, 실제 동작 원리를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- DNS가 필요한 이유
- Name Space
- Name Registration
- Name Resolution
- DNS 설계 목표
- DNS 설계 가정
- HOSTS 파일 방식의 한계
- Domain과 Zone의 차이
- DNS 계층 구조
- Recursive Query와 Iterative Query
- DNS Cache와 TTL
- DNS Resource Record
- DNS가 사용하는 프로토콜(UDP/TCP)
- 실제 DNS 조회 과정

------------------------------------------------------------------------

# 1. DNS란?

DNS(Domain Name System)는 사람이 사용하는 **Domain Name**을 컴퓨터가 사용하는 **IP Address**로 변환하는 **분산형 이름 시스템(Distributed Name System)** 이다.

예를 들어

```text
www.google.com
        ↓
142.250.xxx.xxx
```

사람은 이름(Domain Name)을 기억하고,

컴퓨터는 IP 주소를 이용하여 통신한다.

DNS는 이 둘을 연결해 주는 시스템이다.

------------------------------------------------------------------------

# 2. DNS가 필요한 이유

DNS가 없다면 모든 서버의 IP 주소를 직접 기억해야 한다.

예를 들어

```text
142.250.206.78
```

와 같은 IP 주소를 모두 기억하는 것은 매우 어렵다.

DNS는

```text
www.google.com
```

과 같은 사람이 기억하기 쉬운 이름을

자동으로 IP 주소로 변환해 준다.

------------------------------------------------------------------------

# 3. DNS의 핵심 기능

DNS는 크게 세 가지 기능을 수행한다.

1. Name Space
2. Name Registration
3. Name Resolution

```text
이름 생성
    ↓
이름 등록
    ↓
IP 주소 조회
```

------------------------------------------------------------------------

# 4. Name Space

초기의 네트워크는 **Flat Name Space**를 사용하였다.

```text
HOST1
HOST2
HOST3
HOST4
```

이 방식은 이름 충돌(Name Collision)이 쉽게 발생한다.

DNS는 이를 해결하기 위해 **Hierarchical Name Space(계층형 이름 공간)** 를 사용한다.

```text
.
└── com
    └── google
        └── www
```

계층 구조를 사용하면

- 이름 충돌 감소
- 관리 용이
- 확장성 향상

이라는 장점이 있다.

------------------------------------------------------------------------

# 5. DNS 설계 목표

DNS는 다음 목표를 가지고 설계되었다.

- 전 세계 규모에서도 동작
- 분산 관리
- 지역(Local) 관리
- 높은 확장성
- Cache를 이용한 성능 향상

------------------------------------------------------------------------

# 6. DNS 설계 가정

DNS는 다음과 같은 가정을 기반으로 설계되었다.

- Query가 Update보다 훨씬 많다.
- 이름 변경은 자주 발생하지 않는다.
- 데이터는 분산 관리된다.
- 하나의 서버가 모든 정보를 저장하지 않는다.

------------------------------------------------------------------------

# 7. Name Registration

Name Registration은 이름을 등록하여

다른 사용자가 조회(Name Resolution)할 수 있도록 하는 과정이다.

## ① Table 등록

```text
HOST1 → 192.168.0.10
HOST2 → 192.168.0.20
```

장점

- 구조가 단순
- 구현이 쉬움

단점

- 규모가 커질수록 관리 어려움
- 검색 속도 저하

------------------------------------------------------------------------

## ② Broadcast 등록

```text
HOST1 ?
    ↓
Broadcast
    ↓
HOST1 응답
```

장점

- 별도의 서버 불필요

단점

- 네트워크 트래픽 증가
- 인터넷에서는 사용 불가

대표 사례

- NetBIOS Name Service

------------------------------------------------------------------------

## ③ Database 등록

```text
Name
   ↓
DNS Database
   ↓
IP Address
```

장점

- 대규모 네트워크 관리
- 빠른 검색
- 분산 관리 가능
- 계층적 관리 가능

DNS는 **계층적으로 연결된 분산 데이터베이스(Distributed Database)** 를 사용한다.

------------------------------------------------------------------------

# 8. Name Resolution

```text
www.google.com
        ↓
DNS Query
        ↓
IP Address
```

등록된 이름을 IP 주소로 변환하는 과정을

**Name Resolution**이라고 한다.

------------------------------------------------------------------------

# 9. HOSTS 파일 방식의 한계

HOSTS 파일 방식은 다음과 같은 문제가 있다.

- 파일 크기 증가
- 중앙 관리 필요
- 갱신 비용 증가
- 인터넷 규모로 확장 불가능

이러한 한계를 해결하기 위해 DNS가 등장하였다.

------------------------------------------------------------------------

# 10. Domain과 Zone

- **Domain** : 논리적인 이름 공간
- **Zone** : 하나의 DNS Server가 실제 관리하는 영역

하나의 Domain은

관리 효율을 위해 여러 Zone으로 나누어 운영할 수 있다.

------------------------------------------------------------------------

# 11. DNS 계층 구조

```text
Root DNS
    ↓
TLD DNS
    ↓
Authoritative DNS
    ↓
Host
```

구성 요소

- Root DNS Server
- TLD DNS Server
- Authoritative DNS Server
- Local DNS Resolver

------------------------------------------------------------------------

# 12. 실제 DNS 조회 과정

웹 브라우저에서

```text
www.google.com
```

을 입력하면 다음 순서로 동작한다.

```text
Browser Cache
        ↓
OS DNS Cache
        ↓
HOSTS File
        ↓
Local DNS Resolver
        ↓
(UDP Port 53)

Root DNS
        ↓
TLD DNS
        ↓
Authoritative DNS
        ↓
IP Address 반환
```

IP 주소를 얻은 후,

웹 서버에 접속하는 경우에는

```text
TCP 3-Way Handshake
        ↓
HTTP / HTTPS
```

순으로 통신이 시작된다.

※ HTTP/3는 TCP 대신 **QUIC(UDP 기반)** 을 사용한다.

------------------------------------------------------------------------

# 13. Recursive Query

Client는 Local DNS Resolver에게

최종 IP 주소를 요청한다.

Local DNS가 최종 결과를 찾아 응답하는 방식을

**Recursive Query**라고 한다.

------------------------------------------------------------------------

# 14. Iterative Query

Local DNS Resolver는

Root DNS → TLD DNS → Authoritative DNS를

순차적으로 조회하여

최종 IP 주소를 찾는다.

이를 **Iterative Query**라고 한다.

------------------------------------------------------------------------

# 15. Recursive Query와 Iterative Query의 관계

```text
Client
    │
    │ Recursive Query
    ▼
Local DNS Resolver
    │
    │ Iterative Query
    ▼
Root DNS
    ↓
TLD DNS
    ↓
Authoritative DNS
    ↓
Local DNS Resolver
    ↓
Client
```

즉,

- Client ↔ Local DNS : Recursive Query
- Local DNS ↔ DNS Server : Iterative Query

------------------------------------------------------------------------

# 16. DNS Cache와 TTL

TTL(Time To Live)은

DNS Cache가 유지되는 시간을 의미한다.

TTL이 만료되면

다시 DNS 조회를 수행하여

최신 정보를 가져온다.

장점

- 조회 속도 향상
- Root DNS 부하 감소
- 네트워크 트래픽 감소

------------------------------------------------------------------------

# 17. DNS Resource Record

| Record | 설명 |
|---------|------|
| A | IPv4 주소 |
| AAAA | IPv6 주소 |
| CNAME | 별칭(Alias) |
| NS | 권한 DNS 서버 |
| MX | 메일 서버 |
| PTR | Reverse Lookup |
| SOA | Zone 시작 정보 |

------------------------------------------------------------------------

# 18. DNS는 UDP일까 TCP일까?

DNS는 **기본적으로 UDP Port 53**을 사용한다.

UDP는 연결 설정(3-Way Handshake)이 없어

빠르게 질의(Query)와 응답(Response)을 처리할 수 있기 때문이다.

다만 다음과 같은 경우에는

**TCP Port 53**을 사용한다.

- 응답 데이터가 큰 경우
- UDP 응답이 잘려(Truncated) 다시 요청하는 경우
- Zone Transfer(AXFR, IXFR)
- DNSSEC 등으로 응답 크기가 커지는 경우

| 구분 | 프로토콜 |
|------|----------|
| 일반 DNS 조회 | UDP 53 |
| 큰 응답 | TCP 53 |
| Zone Transfer | TCP 53 |

------------------------------------------------------------------------

# 19. Google Public DNS

Google Public DNS

- 8.8.8.8
- 8.8.4.4

Cloudflare DNS

- 1.1.1.1

------------------------------------------------------------------------

# 20. DNS Message

## Query

Client가 DNS Server에게

IP 주소를 요청하는 메시지

## Response

DNS Server가 Client에게

조회 결과를 반환하는 메시지

Wireshark에서는 다음과 같은 항목을 확인할 수 있다.

- Standard Query
- Standard Query Response

------------------------------------------------------------------------

# 21. Wireshark에서 확인하기

DNS 패킷에서는 다음 항목을 확인할 수 있다.

- Query Name
- Query Type
- Response
- TTL
- IP Address

------------------------------------------------------------------------

# 22. nslookup 실습

```bash
nslookup www.google.com
nslookup www.naver.com
```

------------------------------------------------------------------------

# 교수님 수업 포인트

- Name Space / Registration / Resolution
- HOSTS 파일의 한계
- Root → TLD → Authoritative 조회
- Recursive Query와 Iterative Query
- DNS는 분산 데이터베이스
- DNS Cache와 TTL
- DNS는 기본적으로 UDP 53을 사용한다.

------------------------------------------------------------------------

# 시험 포인트

- Registration 방법 3가지
- Domain과 Zone의 차이
- Recursive Query와 Iterative Query
- DNS Cache와 TTL
- DNS Resource Record
- DNS는 기본적으로 UDP 53을 사용하며, 큰 응답과 Zone Transfer는 TCP 53을 사용한다.

------------------------------------------------------------------------

# 핵심 요약

- DNS는 Domain Name을 IP Address로 변환하는 분산형 이름 시스템이다.
- DNS는 Name Space, Name Registration, Name Resolution 기능을 제공한다.
- DNS는 계층형 Name Space와 분산 데이터베이스를 사용한다.
- DNS는 기본적으로 UDP Port 53을 사용한다.
- 큰 응답이나 Zone Transfer 등의 경우 TCP Port 53을 사용한다.
- 웹 브라우저는 DNS 조회 후 IP 주소를 이용하여 HTTP/HTTPS 또는 HTTP/3 통신을 시작한다.

------------------------------------------------------------------------

# 다음 장

**Chapter 13 - DHCP (Dynamic Host Configuration Protocol)**

- DHCP의 필요성
- DHCP 메시지(DORA)
- Lease
- DHCP Relay
- APIPA
- DHCP Packet 분석
