# TCP FSM (Finite State Machine)

> ⭐ LISTEN = 서버 대기 / ESTABLISHED = 데이터 교환 / TIME-WAIT = 안전 종료 대기

---

## FSM이란?

현재 상태(State)에 따라 다음 상태가 결정되는 구조.
TCP는 항상 하나의 상태를 가지며 이벤트 발생 시 다음 상태로 이동.

---

## TCP 주요 상태 10가지

| 상태 | 설명 |
|------|------|
| CLOSED | 연결이 없는 상태 |
| LISTEN | 서버가 연결 요청을 기다리는 상태 (수동 개방) |
| SYN-SENT | 클라이언트가 SYN 보낸 후 응답 대기 (능동 개방) |
| SYN-RECEIVED | 서버가 SYN 받고 SYN+ACK 보낸 상태 |
| ESTABLISHED | 연결 완료, 데이터 송수신 중 |
| FIN-WAIT-1 | 클라이언트가 FIN 전송, ACK 대기 |
| FIN-WAIT-2 | ACK 받음, 서버의 FIN 대기 |
| CLOSE-WAIT | 서버가 FIN 받은 상태, 아직 보낼 데이터 있을 수 있음 |
| LAST-ACK | 서버가 FIN 보내고 마지막 ACK 대기 |
| TIME-WAIT | 마지막 ACK 보낸 후 일정 시간 대기 |

---

## 연결 수립 상태 전이 (3-Way Handshake)

```
Client                     Server
CLOSED                     CLOSED
    │                          │
    │                          ▼
    │                       LISTEN (수동 개방)
    │
    ▼
SYN-SENT (능동 개방 + SYN 전송)
    │
    │────────── SYN ──────────▶│
    │                          ▼
    │                    SYN-RECEIVED (SYN+ACK 전송)
    │
    │◀───────── SYN+ACK ───────│
    ▼
ESTABLISHED (ACK 전송)
    │
    │────────── ACK ──────────▶│
                               ▼
                         ESTABLISHED
```

---

## 연결 종료 상태 전이 (4-Way Handshake)

```
Client                     Server
ESTABLISHED                ESTABLISHED
    │                          │
    ▼                          │
FIN-WAIT-1 (FIN 전송)          │
    │                          │
    │────────── FIN ──────────▶│
    │                          ▼
    │                    CLOSE-WAIT (ACK 전송)
    │
    │◀────────── ACK ──────────│
    ▼
FIN-WAIT-2                     │
    │                          ▼
    │                    LAST-ACK (FIN 전송)
    │
    │◀────────── FIN ──────────│
    ▼
TIME-WAIT (ACK 전송)
    │
    │────────── ACK ──────────▶│
    │                          ▼
    │                       CLOSED
    ▼ (타이머 완료)
CLOSED
```

---

## TCB (Transmission Control Block)

각 TCP 연결의 데이터를 저장하는 특별한 데이터 구조.

각 연결이 독립적이기 때문에 **연결마다 TCB를 생성**하여 관리.

저장 내용:
- 연결 식별을 위한 두 소켓 번호
- 들어온 데이터 버퍼 포인터
- 나갈 데이터 버퍼 포인터
- SND.UNA, SND.NXT, SND.WND, RCV.NXT, RCV.WND 등

---

## 기본 FSM 개념 정리

| 용어 | 설명 |
|------|------|
| 상태 (State) | 특정 시간에 프로토콜 소프트웨어가 처한 상황 |
| 전이 (Transition) | 한 상태에서 다른 상태로 움직이는 행위 |
| 이벤트 (Event) | 상태 간 전이를 발생시킨 어떤 일 |
| 행동 (Action) | 이벤트에 반응하여 다른 상태로 전이하기 전에 하는 일 |

---

## 자주 틀리는 부분

❌ TIME-WAIT는 오류 상태 → 정상 종료의 일부  
❌ LISTEN은 Client 상태 → Server가 연결 요청을 기다리는 상태  
❌ ESTABLISHED 상태에서는 데이터 송수신만 가능 → FIN으로 종료 시작도 가능
