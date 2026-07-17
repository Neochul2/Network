# TCP 연결 종료 (4-Way Handshake)

> ⭐ FIN → ACK → FIN → ACK / TIME-WAIT = 안전한 종료를 위해 잠시 대기

---

## 왜 4번이나 교환할까?

FIN을 받았다고 바로 끊으면 안 됨. 서버가 아직 보낼 데이터가 남아있을 수 있기 때문.
→ Server의 FIN을 별도로 받아야 정상 종료.

---

## 4-Way Handshake 과정

```
Client                          Server
ESTABLISHED                  ESTABLISHED

① FIN ─────────────────────────▶
   FIN=1

        ACK ◀───────────────────②
        (서버: 아직 보낼 데이터 있을 수 있음)

        FIN ◀───────────────────③
        (서버: 모든 데이터 전송 완료)

④ ACK ─────────────────────────▶

TIME-WAIT                       CLOSED
(일정 시간 후)
CLOSED
```

---

## 단계별 상태 변화

**Client 상태 변화:**
```
ESTABLISHED → FIN-WAIT-1 → FIN-WAIT-2 → TIME-WAIT → CLOSED
```

**Server 상태 변화:**
```
ESTABLISHED → CLOSE-WAIT → LAST-ACK → CLOSED
```

---

## 단계별 설명

| 단계 | 발신 | 플래그 | 의미 |
|------|------|--------|------|
| ① FIN | Client | FIN=1 | 더 이상 보낼 데이터 없음 |
| ② ACK | Server | ACK=1 | FIN 잘 받음, 잠깐 기다려 |
| ③ FIN | Server | FIN=1 | 나도 보낼 데이터 다 보냈음 |
| ④ ACK | Client | ACK=1 | Server FIN 확인 |

---

## TIME-WAIT란?

Client가 마지막 ACK를 보낸 후 바로 연결을 삭제하지 않고 일정 시간 대기하는 상태.

**필요한 이유:**
마지막 ACK가 손실될 수 있음. Server가 FIN을 재전송하면 TIME-WAIT 상태에서 ACK를 다시 보낼 수 있어야 하기 때문.

대기 시간: **MSL(Maximum Segment Lifetime)의 2배**

---

## 3-Way vs 4-Way 비교

| 구분 | 3-Way Handshake | 4-Way Handshake |
|------|----------------|----------------|
| 목적 | 연결 생성 | 연결 종료 |
| 메시지 수 | 3개 | 4개 |
| 대표 Flag | SYN, ACK | FIN, ACK |

---

## 자주 틀리는 부분

❌ FIN을 보내면 즉시 연결 종료 → 상대방도 FIN 보내야 종료  
❌ TIME-WAIT는 오류 상태 → 정상 종료 과정의 일부  
❌ 4-Way도 3-Way처럼 동시에 할 수 있다 → Server의 ACK와 FIN이 분리되는 이유는 아직 데이터가 남아있을 수 있기 때문
