# TCP vs UDP 비교

> ⭐ 시험 직전 1분 복습용

---

## 핵심 비교표

| 항목 | TCP | UDP |
|:---|:---|:---|
| 연결 방식 | 연결형 (Connection-Oriented) | 비연결형 (Connectionless) |
| 신뢰성 | ACK + 재전송 제공 | 제공 안 함 |
| 순서 보장 | O | X |
| 재전송 | O | X |
| 흐름 제어 | Sliding Window | 없음 |
| 혼잡 제어 | O | X |
| 속도 | 상대적으로 느림 | 빠름 |
| 헤더 크기 | 20~60Byte | 8Byte |
| 데이터 단위 | Byte Stream | Message |
| 데이터 양 | 소형~초대형 | 소형~중형 |

---

## TCP 대표 애플리케이션

FTP, Telnet, SMTP, HTTP, POP3, IMAP, BGP, IRC, NFS(나중 버전)

---

## UDP 대표 애플리케이션

DNS, BOOTP, DHCP, TFTP, SNMP, RIP, NFS(초기), VoIP, 온라인 게임, 실시간 스트리밍

---

## Well-Known Port

| Port | 프로토콜 | 서비스 |
|:---:|:---:|:---|
| 21 | TCP | FTP |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | UDP | DNS |
| 69 | UDP | TFTP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 111 | TCP | RPC |
| 138 | UDP | NetBIOS |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 443 | TCP | HTTPS |

---

## 포트 번호 범위

| 구분 | 범위 | 설명 |
|:---|:---:|:---|
| Well-Known | 0~1023 | IANA 표준 서비스 |
| Registered | 1024~49151 | 일반 애플리케이션 |
| Dynamic / Private | 49152~65535 | 클라이언트 임시 포트 |

---

## 자주 틀리는 부분

- ❌ TCP가 항상 더 좋은 프로토콜이다 → **서비스 목적에 따라 선택**
- ❌ UDP는 오류 발생 시 자동 재전송한다 → **재전송은 애플리케이션이 처리**
- ❌ ACK Number는 마지막으로 받은 번호다 → **다음에 받을 Byte 번호**
- ❌ Sequence Number는 Segment 번호다 → **Byte 번호**
