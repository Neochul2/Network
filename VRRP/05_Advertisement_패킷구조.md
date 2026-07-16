# 05. VRRP Advertisement Packet 구조

---

# 학습 목표

이 장에서는 VRRP Advertisement Packet의 구조와 각 필드의 역할을 이해한다.

- VRRP Header 구조를 설명할 수 있다.
- 각 Header 필드의 역할을 이해한다.
- Master와 Backup이 Advertisement를 이용하여 어떻게 통신하는지 이해한다.
- Wireshark에서 VRRP Header를 분석할 수 있다.

---

# VRRP Advertisement Packet이란?

VRRP Advertisement Packet은 Master Router가

"나는 현재 정상적으로 Gateway 역할을 수행하고 있다."

라는 사실을 Backup Router에게 주기적으로 알리는 제어 패킷(Control Packet)이다.

Backup Router는 Advertisement Packet을 계속 수신하면서 Master의 생존 여부를 확인한다.

---

# Advertisement Packet 구조

```text
+--------------------------------------------------+
|                 IP Header                         |
+--------------------------------------------------+
|                 VRRP Header                       |
+--------------------------------------------------+
|            Virtual IP Address List               |
+--------------------------------------------------+
```

VRRP Packet은

IP Header

↓

VRRP Header

↓

Virtual IP Address List

순으로 구성된다.

---

# VRRP Header

```text
+------------------------------------------------------------------+
| Version | Type | VRID | Priority | Count IP Address              |
+------------------------------------------------------------------+
| Auth Type / Reserved | Advertisement Interval | Checksum         |
+------------------------------------------------------------------+
| Virtual IP Address List                                         |
+------------------------------------------------------------------+
```

---

# Header 필드

| 필드 | 설명 |
|------|------|
| Version / Type | VRRP 버전과 Advertisement(Type=1) |
| VRID | Virtual Router 그룹 번호 |
| Priority | Master 우선순위(0~255) |
| Count IP Address | Virtual IP 개수 |
| Auth Type / Reserved | v2는 인증 방식, v3는 Reserved |
| Advertisement Interval | Advertisement 전송 주기 |
| Checksum | 오류 검출 |
| IP Address(es) | Virtual IP 목록 |

---

# Version

VRRP는 Version 2와 Version 3를 사용한다.

Version 2

- IPv4 전용
- Authentication 필드 사용

Version 3

- IPv4 / IPv6 지원
- Authentication 제거
- Advertisement 주기를 100ms 단위까지 지원

---

# Priority

Priority는 Master Router를 결정하는 우선순위 값이다.

값이 클수록 Master가 될 가능성이 높다.

```
255

↓

Virtual IP Owner

150

↓

Master

100

↓

기본값

0

↓

Master 포기
```

---

# Advertisement Interval

Advertisement Interval은 Master Router가 Advertisement Packet을 전송하는 주기이다.

기본값은 1초이다.

---

# Checksum

Checksum은 Packet 전송 중 오류가 발생했는지 검사한다.

Checksum 오류가 발생하면 Packet은 폐기된다.

---

# Virtual IP Address List

Virtual IP Address List에는 VRRP 그룹이 사용하는 Virtual IP가 저장된다.

Backup Router는 이 정보를 이용하여 동일한 Virtual Gateway를 구성한다.

---

# 전송 방식

VRRP Advertisement는 다음과 같은 방식으로 전송된다.

- 목적지 : 224.0.0.18
- IP Protocol Number : 112
- TTL : 255

---

# Packet 흐름

```text
Master

↓

Advertisement Packet 생성

↓

IP Header 추가

↓

VRRP Header 추가

↓

Virtual IP 추가

↓

Multicast 전송

↓

Backup Router 수신
```

---

# v2와 v3 차이

| 항목 | Version 2 | Version 3 |
|------|-----------|-----------|
| 지원 | IPv4 | IPv4 / IPv6 |
| Authentication | 지원 | 제거 |
| Advertisement | 1초 단위 | 100ms 단위 지원 |
| RFC | RFC 3768 | RFC 5798 |

---

# Mermaid 다이어그램

```mermaid
flowchart LR

Master --> Packet[Advertisement Packet]

Packet --> Header[VRRP Header]

Header --> Backup

Backup --> Check[Master 상태 확인]
```

---

# Wireshark에서 확인

```
Protocol

VRRP

↓

Version

3

↓

Type

Advertisement

↓

VRID

10

↓

Priority

150

↓

Advertisement Interval

1 sec

↓

Virtual IP

192.168.10.254
```

---

# 시험 핵심

✔ Advertisement Packet은 Master만 전송한다.

✔ VRID는 Virtual Router를 식별한다.

✔ Priority는 Master를 결정한다.

✔ Advertisement Interval 기본값은 1초이다.

✔ 목적지는 224.0.0.18이다.

✔ Protocol Number는 112이다.

✔ TTL은 255이다.

✔ Version 3는 IPv4와 IPv6를 지원한다.

---

# 암기법

Advertisement

↓

VRRP Header

↓

VRID

↓

Priority

↓

Advertisement Interval

↓

Checksum

↓

Virtual IP

---

# 면접 질문

Q. Advertisement Packet의 역할은 무엇인가?

Q. VRRP Header에는 어떤 정보가 포함되는가?

Q. Version 2와 Version 3의 차이는 무엇인가?

Q. Advertisement Interval의 기본값은 얼마인가?

Q. Checksum의 역할은 무엇인가?

---

# 핵심 요약

VRRP Advertisement Packet은 Master Router가 자신의 생존 상태를 Backup Router에게 알리기 위한 제어 패킷이다.

Packet에는 Version, VRID, Priority, Advertisement Interval, Checksum, Virtual IP 등의 정보가 포함되며, Backup Router는 이를 이용하여 Master의 상태를 지속적으로 감시한다.

Version 3는 IPv4와 IPv6를 지원하며, RFC 5798에서 정의된 최신 규격이다.