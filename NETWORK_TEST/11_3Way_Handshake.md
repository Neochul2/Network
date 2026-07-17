# TCP 연결 수립 (3-Way Handshake)

> ⭐ SYN → SYN+ACK → ACK / ISN = 32비트 초기 순서 번호

---

## 왜 연결이 필요한가?

데이터를 보내기 전에 확인해야 할 것들:
- 상대방이 존재하는가?
- 데이터 받을 준비가 됐는가?
- 초기 Sequence Number(ISN)는 무엇인가?

---

## 3-Way Handshake 과정

```
Client                          Server
CLOSED                          LISTEN

① SYN ─────────────────────────▶
   SYN=1, Seq=4567

        SYN+ACK ◀───────────────②
        SYN=1, ACK=1
        Ack=4568, Seq=12998

③ ACK ─────────────────────────▶
   ACK=1, Ack=12999

ESTABLISHED                  ESTABLISHED
```

---

## 단계별 설명

| 단계 | 발신 | 수신 | 플래그 | 상태 변화 |
|------|------|------|--------|-----------|
| ① SYN | Client | Server | SYN=1 | CLOSED → SYN-SENT |
| ② SYN+ACK | Server | Client | SYN=1, ACK=1 | LISTEN → SYN-RECEIVED |
| ③ ACK | Client | Server | ACK=1 | SYN-SENT → ESTABLISHED |
| (완료) | - | - | - | SYN-RECEIVED → ESTABLISHED |

---

## ISN (Initial Sequence Number) 동기화

각 TCP 장비는 연결 초기화 시 **32비트 ISN**을 선택.

```
① SYN      : Seq = 4,567 (Client ISN)
② SYN+ACK  : Ack = 4,568 (Client ISN+1)
             Seq = 12,998 (Server ISN)
③ ACK      : Ack = 12,999 (Server ISN+1)
```

- ISN은 무작위로 선택 (보안상 예측 어렵게)
- 양측이 서로의 ISN을 교환하여 **동기화** 완료

---

## 연결 수립의 3가지 기능

1. **접촉과 통신** - 클라이언트와 서버가 서로 접촉하여 통신 시작 확인
2. **순서 번호 동기화** - 각 장비가 첫 번째 통신에 사용할 ISN 교환
3. **인자 교환** - TCP 연결 동작 제어를 위한 인자(MSS 등) 교환

---

## 왜 3번이나 통신할까?

2번만 하면 Server가 Client가 응답을 받았는지 확인 불가.
3번째 ACK가 있어야 **양쪽 모두** 연결 성공 확인 가능.

---

## 자주 틀리는 부분

❌ TCP 연결 종료도 3-Way → 연결 종료는 4-Way Handshake  
❌ ISN은 항상 1부터 시작 → 32비트 무작위 값  
❌ 2번 교환으로 충분하다 → Server도 Client가 응답 받았는지 확인 필요
