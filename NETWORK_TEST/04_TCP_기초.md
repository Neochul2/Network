# TCP 기초

> ⭐ TCP = 연결형 + 신뢰성 + Byte Stream + 흐름제어

---

## TCP란?

전송계층의 연결형(Connection-Oriented) 프로토콜. 데이터를 정확하고 신뢰성 있게 전달하는 것이 목적.

---

## TCP 제공 기능

1. 주소지정 / 다중화
2. 연결 수립, 유지, 종료
3. 데이터 처리와 패키징
4. 데이터 전송
5. 신뢰성과 품질 서비스 제공
6. 흐름 제어와 혼잡 회피

---

## TCP가 수행하지 않는 기능

1. 애플리케이션 사용 명시
2. 보안 제공 (암호화는 TLS/SSL)
3. 메시지 경계 유지
4. 통신 보장

---

## TCP 8가지 특성

| 특성 | 설명 |
|------|------|
| 연결형 | 3-Way Handshake 후 데이터 전송 |
| 양방향 | 동시 송수신 가능 (Full Duplex) |
| 다중화 | Port 번호로 프로그램 구분 |
| 신뢰성 | 손실·중복·순서 문제 해결 |
| 승인(ACK) | 수신 확인 메시지 전송 |
| Byte Stream 기반 | Byte 단위로 관리 |
| 구조화되지 않은 데이터 | 데이터 의미를 해석하지 않음 |
| 흐름 제어 | Window Size로 송신량 조절 |

---

## TCP 스트림 동작

TCP는 데이터를 **Byte Stream**으로 관리한다. 메시지 단위가 아님.

```
Application → "HELLO" 전송

TCP에서는:
H(1) E(2) L(3) L(4) O(5) → 각 Byte에 순서 번호 부여
```

- IP는 메시지(Packet) 중심 → TCP는 이를 Segment로 분할하여 전달
- 수신 측은 Segment를 재조립하여 Byte Stream 복원
- 이 분할 단위를 **TCP 세그먼트(Segment)** 라고 함

---

## ACK 동작 원리

```
1~100 Byte 수신 완료
→ ACK = 101  (다음에 받을 Byte 번호)
```

**ACK는 마지막으로 받은 번호가 아니라 다음에 받을 번호!**

---

## TCP 대표 서비스

HTTP, HTTPS, FTP, SSH, SMTP, POP3, IMAP, Telnet, BGP

---

## 자주 틀리는 부분

❌ TCP는 Message를 관리한다 → Byte Stream을 관리  
❌ TCP는 데이터 의미를 해석한다 → Byte Stream만 처리, 의미는 Application이 해석  
❌ TCP는 암호화를 제공한다 → 암호화는 TLS/SSL  
❌ ACK Number는 마지막으로 받은 번호다 → 다음에 받아야 할 번호
