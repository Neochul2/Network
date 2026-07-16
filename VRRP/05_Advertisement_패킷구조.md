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

VRRP Advertisement Packet은

Master Router가

"나는 현재 정상적으로 Gateway 역할을 수행하고 있다."

라는 사실을 Backup Router에게 주기적으로 알리는 제어 패킷(Control Packet)이다.

Backup Router는 Advertisement Packet을 계속 수신하면서

Master의 생존 여부를 확인한다.

---

# Advertisement Packet 구조

```text
+--------------------------------------------------+
|                 IP Header                         |
+--------------------------------------------------+
|              VRRP Header                          |
+--------------------------------------------------+
|          Virtual IP Address List                 |
+--------------------------------------------------+
```

VRRP Packet은

IP Header

↓

VRRP Header

↓

Virtual IP 목록

순으로 구성된다.

---

# VRRP Header

VRRP Header는

Master Router를 식별하고

VRRP 그룹 정보를 전달하는 역할을 한다.

구성은 다음과 같다.

```text
+--------------------------------------------------------------+
| Version | Type | VRID | Priority | Count IP Address          |
+--------------------------------------------------------------+
| Advertisement Interval | Checksum                           |
+--------------------------------------------------------------+
| Virtual IP Address List                                     |
+--------------------------------------------------------------+
```

---

# Version

VRRP 프로토콜의 버전을 나타낸다.

대표적으로

VRRP Version 2

VRRP Version 3

를 사용한다.

현재는

Version 3가 가장 많이 사용된다.

---

# Type

Packet의 종류를 나타낸다.

현재 VRRP에서는

Advertisement

한 가지 Type만 사용한다.

즉,

VRRP Packet은

모두 Advertisement Packet이다.

---

# VRID (Virtual Router Identifier)

VRRP 그룹 번호이다.

같은 그룹의 Router는

동일한 VRID를 사용한다.

예)

VRID = 10

↓

Router A

↓

Router B

↓

같은 Virtual Router

---

# Priority

Master Router를 결정하는 우선순위 값이다.

값이 클수록

Master가 될 가능성이 높다.

예)

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

---

# Count IP Address

Advertisement Packet 안에 포함된

Virtual IP의 개수를 의미한다.

일반적으로는

1개

사용한다.

---

# Advertisement Interval

Master Router가

Advertisement Packet을 보내는 간격이다.

기본값은

1초

이다.

즉,

Master는

1초마다 자신의 생존을 알린다.

---

# Checksum

Packet 전송 중

오류가 발생했는지 검사한다.

Checksum이 틀리면

Packet은 폐기된다.

---

# Virtual IP Address List

VRRP 그룹이 사용하는

Virtual IP를 저장한다.

예)

192.168.10.254

Backup Router는

이 값을 이용하여

동일한 Virtual Gateway를 구성한다.

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

# Packet 분석

Advertisement Packet에는

다음 정보가 모두 포함된다.

├─ Version

├─ Type

├─ VRID

├─ Priority

├─ Advertisement Interval

├─ Checksum

└─ Virtual IP

Backup Router는

이 정보를 이용하여

Master 상태를 판단한다.

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

VRRP Packet을 캡처하면

다음과 같은 항목을 확인할 수 있다.

Protocol

↓

VRRP

Version

↓

3

Type

↓

Advertisement

Priority

↓

150

VRID

↓

10

Advertisement Interval

↓

1 sec

Virtual IP

↓

192.168.10.254

---

# 시험 핵심

✔ VRRP는 Advertisement Packet으로 상태를 알린다.

✔ Advertisement Packet은 Master만 전송한다.

✔ VRID는 Virtual Router를 식별한다.

✔ Priority는 Master를 결정한다.

✔ Advertisement Interval 기본값은 1초이다.

✔ Checksum은 오류를 검사한다.

✔ Virtual IP는 Gateway 주소이다.

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

Q. VRRP Advertisement Packet의 역할은 무엇인가?

Q. VRID는 왜 필요한가?

Q. Advertisement Interval의 기본값은 얼마인가?

Q. Checksum의 역할은 무엇인가?

Q. Backup Router는 Advertisement Packet을 어떻게 활용하는가?

---

# 핵심 요약

VRRP Advertisement Packet은 Master Router가 자신의 생존 상태를 Backup Router에게 알리기 위한 제어 패킷이다.

Packet에는 VRID, Priority, Advertisement Interval, Virtual IP 등의 정보가 포함되며, Backup Router는 이를 이용하여 Master의 상태를 지속적으로 감시한다.