# DNS — 왜 필요한가부터 어떻게 동작하는가까지

---

## 1. 문제: 사람은 숫자를 기억하기 어렵다

인터넷에서 서버를 찾으려면 IP 주소가 필요하다.

```
구글 서버: 142.250.206.78
네이버 서버: 223.130.200.104
유튜브 서버: 142.250.207.14
```

하지만 사람은 이런 숫자를 수백 개 기억할 수 없다.

그래서 사람이 기억하기 쉬운 **이름(Domain Name)** 을 쓰기 시작했다.

```
www.google.com
www.naver.com
www.youtube.com
```

---

## 2. 초기 해결책: HOSTS 파일

초기 인터넷(ARPAnet)에서는 간단하게 해결했다.
모든 컴퓨터에 **HOSTS 파일** 하나를 배포했다.

```
142.250.206.78    www.google.com
223.130.200.104   www.naver.com
```

이 파일을 보고 이름 → IP 주소로 변환했다.
컴퓨터가 수십 대일 때는 괜찮았다.

---

## 3. 문제: HOSTS 파일 방식의 한계

인터넷이 폭발적으로 성장하면서 문제가 생겼다.

| 문제점 | 설명 |
|--------|------|
| 파일 크기 증가 | 수백만 개의 호스트를 하나의 파일에 담을 수 없음 |
| 중앙 관리 필요 | 누군가 한 곳에서 관리해야 하는데 불가능한 규모 |
| 갱신 비용 증가 | 새 서버가 생길 때마다 전 세계 파일을 업데이트해야 함 |
| 확장 불가 | 인터넷 규모로는 근본적으로 불가능 |

또한 HOSTS 파일은 **단순 구조(Flat Name Space)** 였다.

```
HOST1, HOST2, HOST3, HOST4 ...
```

이름 충돌(Name Collision)이 쉽게 발생했다.
"mail"이라는 이름을 두 회사가 동시에 쓰면 충돌이 난다.

---

## 4. 해결책의 조건

새로운 이름 시스템이 필요했다. 요구사항은 다음과 같았다.

- 전 세계 수백만 대 이상을 수용할 수 있어야 한다
- 중앙 한 곳이 아니라 **분산 관리** 가 가능해야 한다
- 이름 충돌이 없어야 한다
- 빠르게 조회할 수 있어야 한다

이 요구사항을 모두 만족하는 시스템이 **DNS(Domain Name System)** 다.

---

## 5. DNS의 첫 번째 해결: 계층형 이름 공간

DNS는 이름을 **계층 구조(Hierarchical Name Space)** 로 만들었다.

```
.                      ← Root (최상위)
└── com
    └── google
        └── www        ← www.google.com
```

이렇게 하면:
- 각 조직이 자기 영역 안에서 자유롭게 이름을 붙일 수 있다
- 전체 이름(FQDN)이 다르면 충돌이 없다
- google.com의 "mail"과 naver.com의 "mail"은 서로 다른 이름이다

---

## 6. DNS의 두 번째 해결: 이름 등록 방식

이름을 등록하는 방법은 3가지가 있다.

### ① Table 등록 (HOSTS 파일 방식)
관리자가 직접 테이블에 이름-IP를 기록.
단순하지만 규모가 커지면 한계.

### ② Broadcast 등록
"이 이름 쓰는 사람 있어?" 하고 네트워크 전체에 물어보는 방식.
별도 서버가 필요 없지만 인터넷 규모에서는 불가능.
→ 대표 사례: **NetBIOS Name Service**

### ③ Database 등록 ← DNS가 채택한 방식
이름을 **분산 데이터베이스** 에 등록.
계층적으로 권한을 위임하여 각 조직이 자기 영역을 관리.
전 세계 규모로 확장 가능.

---

## 7. DNS의 세 번째 해결: 분산 데이터베이스 구조

DNS는 하나의 서버가 모든 정보를 갖지 않는다.
**계층적으로 나뉜 여러 서버** 가 각자 담당 영역만 관리한다.

```
Root DNS Server         ← "." 전체를 담당
      ↓
TLD DNS Server          ← ".com", ".kr" 등 담당
      ↓
Authoritative DNS       ← "google.com" 담당
      ↓
Host                    ← 최종 IP 주소
```

| 서버 | 역할 |
|------|------|
| Root DNS | 전체 DNS 트리의 시작점. TLD 서버 위치를 안다 |
| TLD DNS | .com, .net, .kr 등 최상위 도메인 관리 |
| Authoritative DNS | 실제 도메인(google.com)의 IP를 가진 서버 |
| Local DNS Resolver | 클라이언트 대신 조회를 수행하는 서버 |

---

## 8. Domain과 Zone의 차이

혼동하기 쉬운 두 개념이다.

| 구분 | 설명 |
|------|------|
| Domain | 논리적인 이름 공간 (google.com 전체) |
| Zone | 하나의 DNS 서버가 실제 관리하는 영역 |

google.com이라는 Domain은 관리 편의를 위해 여러 Zone으로 나뉠 수 있다.

---

## 9. 실제 DNS 조회 흐름

브라우저에 `www.google.com`을 입력하면 어떤 일이 벌어질까?

```
① Browser Cache 확인
        ↓ (없으면)
② OS DNS Cache 확인
        ↓ (없으면)
③ HOSTS 파일 확인
        ↓ (없으면)
④ Local DNS Resolver에 질의 (UDP Port 53)
        ↓
⑤ Root DNS → "com을 담당하는 TLD 서버 주소는 이거야"
        ↓
⑥ TLD DNS → "google.com을 담당하는 서버 주소는 이거야"
        ↓
⑦ Authoritative DNS → "www.google.com의 IP는 142.250.xxx.xxx야"
        ↓
⑧ IP 주소를 브라우저에 반환
        ↓
⑨ TCP 3-Way Handshake → HTTP/HTTPS 연결 시작
```

---

## 10. Recursive Query vs Iterative Query

조회 방식에는 두 가지가 있다.

**Recursive Query (재귀 질의)**
- Client → Local DNS: "www.google.com의 IP 알려줘"
- Local DNS가 끝까지 찾아서 최종 답을 돌려준다
- Client 입장에서는 Local DNS에 한 번만 물어보면 된다

**Iterative Query (반복 질의)**
- Local DNS → Root DNS: "google.com 어디 있어?" → "TLD 서버 물어봐"
- Local DNS → TLD DNS: "google.com 어디 있어?" → "Authoritative 물어봐"
- Local DNS → Authoritative DNS: "www.google.com IP?" → "142.250.xxx.xxx"
- Local DNS가 직접 발로 뛰어다닌다

```
Client ──[Recursive]──▶ Local DNS Resolver
                               │
                         [Iterative]
                               ↓
                           Root DNS
                               ↓
                           TLD DNS
                               ↓
                       Authoritative DNS
                               ↓
                         Local DNS → Client
```

즉:
- **Client ↔ Local DNS = Recursive**
- **Local DNS ↔ DNS 서버들 = Iterative**

---

## 11. 성능 문제: 매번 조회하면 느리다 → Cache와 TTL

DNS 조회는 여러 서버를 거치기 때문에 느릴 수 있다.
그래서 **Cache** 를 사용한다.

한 번 조회한 결과를 일정 시간 저장해 두고 재사용한다.

이 유지 시간을 **TTL(Time To Live)** 라고 한다.

TTL이 만료되면 다시 조회하여 최신 정보를 가져온다.

Cache의 장점:
- 조회 속도 향상
- Root DNS 부하 감소
- 네트워크 트래픽 감소

---

## 12. DNS가 사용하는 프로토콜: 왜 UDP인가?

DNS는 기본적으로 **UDP Port 53** 을 사용한다.

이유: DNS 질의는 짧은 요청 → 짧은 응답이다.
TCP처럼 3-Way Handshake를 할 필요 없이 바로 주고받는 게 빠르다.

단, 다음 경우에는 **TCP Port 53** 을 사용한다.

| 상황 | 이유 |
|------|------|
| 응답 데이터가 큰 경우 | UDP는 512Byte(기본) 초과 시 잘림 |
| Zone Transfer | 서버 간 대량 데이터 동기화 |
| DNSSEC | 보안 서명으로 응답이 커짐 |

---

## 13. DNS Resource Record — 무엇을 저장하는가?

DNS는 IP 주소만 저장하는 게 아니다.

| Record | 설명 |
|--------|------|
| A | 도메인 → IPv4 주소 |
| AAAA | 도메인 → IPv6 주소 |
| CNAME | 도메인 → 다른 도메인 (별칭) |
| MX | 메일 서버 주소 |
| NS | 이 도메인을 담당하는 DNS 서버 |
| PTR | IP → 도메인 (역방향 조회) |
| SOA | Zone의 시작 정보 |

---

## 14. 핵심 요약 — 전체 흐름 한눈에

```
사람: "www.google.com 접속하고 싶어"
        ↓
문제: IP 주소를 모른다
        ↓
HOSTS 파일 방식 → 인터넷 규모에서 한계
        ↓
DNS 등장: 계층형 이름 공간 + 분산 데이터베이스
        ↓
조회: Client → Local DNS (Recursive)
           Local DNS → Root/TLD/Authoritative (Iterative)
        ↓
결과: IP 주소 획득 → TCP 연결 → HTTP 통신 시작
        ↓
성능: Cache + TTL로 반복 조회 줄임
        ↓
프로토콜: 기본 UDP 53 / 대용량·Zone Transfer는 TCP 53
```

---

## 자주 틀리는 부분

❌ DNS는 항상 TCP → 기본은 UDP 53  
❌ Domain = Zone → Domain은 논리적 공간, Zone은 실제 관리 영역  
❌ Recursive = Iterative → Client↔LocalDNS는 Recursive, LocalDNS↔서버는 Iterative  
❌ DNS는 IP만 저장 → A, AAAA, MX, CNAME 등 다양한 Record 저장
