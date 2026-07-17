# DNS (Domain Name System)

> ⭐ DNS = 분산형 이름 시스템 / 기본 UDP 53 / Zone Transfer는 TCP 53

---

## DNS란?

사람이 사용하는 **Domain Name**을 컴퓨터가 사용하는 **IP Address**로 변환하는 분산형 이름 시스템.

```
www.google.com → 142.250.xxx.xxx
```

---

## DNS 핵심 기능 3가지

| 기능 | 설명 |
|------|------|
| Name Space | 이름 공간 정의 |
| Name Registration | 이름 등록 |
| Name Resolution | 이름→IP 변환 |

---

## Name Space

**단순 구조 (Flat Name Space):**
```
HOST1, HOST2, HOST3 ...
```
- 이름 충돌(Name Collision) 쉽게 발생
- 한 장치는 다른 장치들과 논리적으로 동등

**계층 구조 (Hierarchical Name Space):**
```
.
└── com
    └── google
        └── www
```
- 이름 충돌 감소
- 관리 용이
- 확장성 향상

---

## Name Registration 3가지 방법

이름을 등록하여 다른 사용자가 조회(Name Resolution)할 수 있도록 하는 과정.

### ① Table 등록

```
HOST1 → 192.168.0.10
HOST2 → 192.168.0.20
```

한 명의 관리자가 하나의 테이블을 가지고 이름 지정을 운용.
이름 추가/삭제/변경은 테이블 수정으로 이루어짐.

| 장점 | 단점 |
|------|------|
| 구조가 단순 | 규모가 커질수록 관리 어려움 |
| 구현이 쉬움 | 검색 속도 저하 |

### ② Broadcast 등록

시행착오(Trial-and-Error) 방법 사용.
한 장치가 특정 이름을 사용하려면 먼저 네트워크상의 모든 장치에 브로드캐스트로 이미 그 이름이 사용 중인지 확인.

```
HOST1 ?
    ↓
Broadcast (전체 네트워크에 질의)
    ↓
HOST1 응답 (이미 있으면 충돌)
```

| 장점 | 단점 |
|------|------|
| 별도 서버 불필요 | 네트워크 트래픽 증가 |
| 구현 단순 | 인터넷 규모에서는 사용 불가 |

대표 사례: **NetBIOS Name Service**

### ③ Database 등록

네임을 등록하기 위해 데이터베이스에 추가할 이름 지정 요청을 작성.
네임 체계 기관이 완전히 중앙 집중적이라면 데이터베이스도 그 기관이 관리.

```
Name
   ↓
DNS Database
   ↓
IP Address
```

| 장점 | 단점 |
|------|------|
| 대규모 네트워크 관리 가능 | 구현 복잡 |
| 빠른 검색 | 초기 구축 비용 |
| 분산/계층적 관리 가능 | |

DNS는 **계층적으로 연결된 분산 데이터베이스(Distributed Database)** 를 사용한다.

---

## Name Resolution 3가지 방법

등록된 이름을 IP 주소로 변환하는 과정.

### ① Table 기반 네임 변환

네트워크의 각 장치가 Table 등록에 사용하는 테이블을 직접 조회하여 이름→주소 변환.
→ HOSTS 파일 방식이 이에 해당.

### ② Broadcast 네임 변환

Broadcast 네임 등록과 상호보완적.
모든 장치가 브로드캐스트 가능한 간단한 시스템에서만 사용 가능.
→ 인터넷 규모에서는 불가.

### ③ Client/Server 네임 변환

서버 소프트웨어가 클라이언트가 보낸 네임 변환 요청에 응답.
→ DNS가 이 방식을 사용.

```
Client → DNS Server에 질의 → IP 주소 응답
```

| 방법 | 설명 | 대표 사례 |
|------|------|-----------|
| Table 기반 | 로컬 테이블 직접 조회 | HOSTS 파일 |
| Broadcast | 전체 네트워크에 브로드캐스트 질의 | NetBIOS |
| Client/Server | DNS 서버에 질의하여 응답 | DNS |

---

## DNS 설계 목표

- 전 세계 규모에서도 동작
- 분산 관리 / 지역(Local) 관리
- 높은 확장성
- Cache를 이용한 성능 향상

---

## DNS 설계 가정

- Query가 Update보다 훨씬 많다
- 이름 변경은 자주 발생하지 않는다
- 데이터는 분산 관리된다
- 하나의 서버가 모든 정보를 저장하지 않는다

---

## HOSTS 파일 방식의 한계

| 문제점 |
|--------|
| 파일 크기 증가 |
| 중앙 관리 필요 |
| 갱신 비용 증가 |
| 인터넷 규모로 확장 불가능 |

→ 이 한계를 해결하기 위해 DNS 등장

---

## Domain vs Zone

| 구분 | 설명 |
|------|------|
| Domain | 논리적인 이름 공간 |
| Zone | 하나의 DNS Server가 실제 관리하는 영역 |

하나의 Domain은 관리 효율을 위해 여러 Zone으로 나누어 운영 가능.

---

## DNS 계층 구조

```
Root DNS Server
      ↓
TLD DNS Server (.com, .kr 등)
      ↓
Authoritative DNS Server (google.com 담당)
      ↓
Host
```

---

## DNS 조회 과정

```
Browser Cache
      ↓
OS DNS Cache
      ↓
HOSTS File
      ↓
Local DNS Resolver (UDP Port 53)
      ↓
Root DNS → TLD DNS → Authoritative DNS
      ↓
IP Address 반환
      ↓
TCP 3-Way Handshake → HTTP/HTTPS 연결
```

※ HTTP/3는 TCP 대신 QUIC(UDP 기반) 사용

---

## Recursive Query vs Iterative Query

| 구분 | 설명 |
|------|------|
| Recursive Query | Client → Local DNS: 최종 IP 주소 요청, Local DNS가 끝까지 찾아서 응답 |
| Iterative Query | Local DNS → Root/TLD/Authoritative: 순차 조회하여 IP 찾음 |

```
Client ─[Recursive]─▶ Local DNS Resolver
                           │
                     [Iterative]
                           ↓
                       Root DNS → TLD DNS → Authoritative DNS
                           ↓
                       Local DNS → Client
```

---

## DNS Cache와 TTL

**TTL (Time To Live):** DNS Cache가 유지되는 시간.

TTL 만료 시 다시 DNS 조회 수행.

장점:
- 조회 속도 향상
- Root DNS 부하 감소
- 네트워크 트래픽 감소

---

## DNS Resource Record

| Record | 설명 |
|--------|------|
| A | IPv4 주소 |
| AAAA | IPv6 주소 |
| CNAME | 별칭(Alias) |
| NS | 권한 DNS 서버 |
| MX | 메일 서버 |
| PTR | Reverse Lookup (IP→이름) |
| SOA | Zone 시작 정보 |

---

## DNS 프로토콜 (UDP vs TCP)

| 구분 | 프로토콜 |
|------|----------|
| 일반 DNS 조회 | **UDP 53** (기본) |
| 큰 응답 | TCP 53 |
| Zone Transfer (AXFR, IXFR) | TCP 53 |
| DNSSEC 등 응답 크기 큰 경우 | TCP 53 |

UDP 사용 이유: 연결 설정(3-Way Handshake) 없어 빠른 질의/응답 가능.

---

## 공개 DNS 서버

| 서비스 | 주소 |
|--------|------|
| Google Public DNS | 8.8.8.8 / 8.8.4.4 |
| Cloudflare DNS | 1.1.1.1 |

---

## 자주 틀리는 부분

❌ DNS는 항상 TCP를 사용 → 기본은 UDP 53, 큰 응답/Zone Transfer만 TCP 53  
❌ Domain과 Zone은 같은 개념 → Domain은 논리적 공간, Zone은 실제 관리 영역  
❌ Recursive Query = Iterative Query → Client↔LocalDNS는 Recursive, LocalDNS↔서버는 Iterative
