# TCP/IP Advanced

# Chapter 12
# DNS (Domain Name System)

---

# 학습 목표

이번 장에서는 교수님 PDF를 기반으로 DNS(Domain Name System)의 설계 철학과 실제 동작 원리를 학습한다.

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
- DNS Record
- 실제 DNS 조회 과정

---

# 1. DNS란?

DNS(Domain Name System)는 사람이 사용하는 Domain Name을 컴퓨터가 사용하는 IP Address로 변환하는 분산형 이름 시스템(Distributed Name System)이다.

예)

```text
www.google.com
↓
142.250.xxx.xxx
```

사람은 이름을 기억하고, 컴퓨터는 IP 주소를 이용하여 통신한다.

DNS는 둘을 연결하는 시스템이다.

---

# 2. DNS가 필요한 이유

DNS가 없다면 모든 서버의 IP 주소를 직접 기억해야 한다.

예)

```text
142.250.206.78
```

하지만 `www.google.com`처럼 이름을 사용하는 것이 훨씬 편리하다.

DNS는 이름을 IP 주소로 자동 변환한다.

---

# 3. DNS의 핵심 기능

1. Name Space
2. Name Registration
3. Name Resolution

```text
이름 생성
 ↓
등록
 ↓
IP 조회
```

---

# 4. Name Space

초기의 네트워크는 Flat Name Space를 사용하였다.

```text
HOST1
HOST2
HOST3
HOST4
```

DNS는 이름 충돌 문제를 해결하기 위해 Hierarchical Name Space를 사용한다.

```text
.
└── com
    └── google
        └── www
```

---

# 5. DNS 설계 목표

- 전 세계 규모에서도 동작
- 분산 관리
- 지역(Local) 관리
- 높은 확장성
- Cache를 이용한 성능 향상

---

# 6. DNS 설계 가정

- Query가 Update보다 훨씬 많다.
- 이름 변경은 자주 발생하지 않는다.
- 데이터는 분산 관리된다.
- 하나의 서버가 모든 정보를 저장하지 않는다.

---

# 7. Name Registration

Name Registration은 이름을 등록하여 다른 사용자가 조회(Name Resolution)할 수 있도록 하는 과정이다.

## ① 테이블(Table) 등록

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

## ② 브로드캐스트(Broadcast) 등록

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

## ③ 데이터베이스(Database) 등록

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

DNS는 계층적으로 연결된 분산 데이터베이스(Distributed Database)를 사용한다.

---

# 8. Name Resolution

```text
www.google.com
 ↓
DNS Query
 ↓
IP Address
```

---

# 9. HOSTS 파일 방식의 한계

- 파일 크기 증가
- 중앙 관리
- 갱신 비용 증가
- 인터넷 확장 불가능

---

# 10. Domain과 Zone

- Domain : 논리적인 이름 공간
- Zone : 하나의 DNS Server가 실제 관리하는 영역

하나의 Domain은 여러 Zone으로 분할될 수 있다.

---

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

- Root DNS
- TLD DNS
- Authoritative DNS
- Local DNS Resolver

---

# 12. 실제 DNS 조회 과정

```text
Browser Cache
 ↓
OS Cache
 ↓
HOSTS File
 ↓
Local DNS
 ↓
Root DNS
 ↓
TLD DNS
 ↓
Authoritative DNS
 ↓
IP Address
 ↓
TCP 3-Way Handshake
 ↓
HTTP / HTTPS
```

---

# 13. Recursive Query

Client는 Local DNS에게 최종 IP 주소를 요청한다.

---

# 14. Iterative Query

Local DNS는 Root → TLD → Authoritative DNS를 순차적으로 조회한다.

---

# 15. Recursive와 Iterative의 관계

```text
Client
 ↓ (Recursive Query)
Local DNS
 ↓ (Iterative Query)
Root
 ↓
TLD
 ↓
Authoritative
 ↓
Local DNS
 ↓
Client
```

Client는 Recursive Query를 사용하고, Local DNS는 Iterative Query를 수행하여 최종 IP를 찾는다.

---

# 16. DNS Cache와 TTL

TTL(Time To Live)이 지나면 Cache를 갱신하기 위해 다시 조회한다.

장점
- 조회 속도 향상
- Root DNS 부하 감소

---

# 17. DNS Resource Record

| Record | 설명 |
|---------|------|
| A | IPv4 주소 |
| AAAA | IPv6 주소 |
| CNAME | 별칭 |
| NS | 권한 DNS 서버 |
| MX | 메일 서버 |
| PTR | Reverse Lookup |
| SOA | Zone 시작 정보 |

---

# 18. Google Public DNS

- 8.8.8.8
- 8.8.4.4

Cloudflare

- 1.1.1.1

---

# 19. DNS Message

## Query

Client가 DNS Server에게 IP 주소를 요청하는 메시지

## Response

DNS Server가 Client에게 조회 결과를 반환하는 메시지

Wireshark에서는 Standard Query / Standard Query Response를 확인할 수 있다.

---

# 20. Wireshark

- Query Name
- Query Type
- Response
- TTL
- IP Address

---

# 21. nslookup 실습

```bash
nslookup www.google.com
nslookup www.naver.com
```

---

# 교수님 수업 포인트

- Name Space / Registration / Resolution
- HOSTS 파일의 한계
- Root → TLD → Authoritative 조회
- DNS는 분산 데이터베이스
- Cache와 TTL

---

# 시험 포인트

- Registration 방법 3가지
- Domain과 Zone 차이
- Recursive / Iterative Query
- Cache와 TTL

---

# 핵심 요약

- DNS는 분산형 Name System이다.
- Name Space, Registration, Resolution을 제공한다.
- Registration은 Table, Broadcast, Database 방식이 있다.
- DNS는 계층형 Name Space와 분산 데이터베이스를 사용한다.
- DNS 조회 완료 후 TCP 연결이 시작된다.
