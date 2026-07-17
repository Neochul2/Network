# SACK (Selective Acknowledgement)

> ⭐ SACK = 손실된 Segment만 재전송 / TCP Options 필드 사용 / RFC 2018

---

## SACK란?

수신자가 이미 정상적으로 받은 데이터 구간을 송신자에게 알려주는 기능.

**기본 ACK:** "여기까지 받았습니다."
**SACK:** "여기는 이미 받았고, 여기만 다시 보내세요."

---

## 누적 ACK의 문제점

```
전송: 1 2 3 4 5
수신: 1✓ 2✓ 3✗ 4✓ 5✓

수신자는 계속 ACK=3 만 보냄
송신자는 4, 5를 받았는지 모름
→ 3, 4, 5 모두 재전송할 수도 있음
```

결과: 대역폭 낭비, 네트워크 혼잡, 처리량 감소

---

## SACK 동작 과정

```
Step 1: 송신자가 1 2 3 4 5 전송
Step 2: 3번 손실 → 1✓ 2✓ 3✗ 4✓ 5✓
Step 3: 수신자가 ACK=3 + SACK(4~5 수신 완료) 전송
Step 4: 송신자는 3만 재전송
```

---

## 누적 ACK vs SACK 비교

| 구분 | 누적 ACK | SACK |
|------|----------|------|
| 승인 방식 | 누적 승인 | 선택적 승인 |
| 손실 확인 | 손실 여부만 확인 | 수신 구간까지 확인 |
| 재전송 | 여러 Segment 재전송 가능 | 손실 Segment만 재전송 |
| 대역폭 | 비효율적 | 효율적 |
| 처리량 | 보통 | 우수 |
| TCP Option 사용 | X | O |

---

## SACK 저장 위치

**TCP Header의 Options 필드**에 저장. 기본 20Byte Header에는 없음.

```
Options (0~40Byte) + Padding ← 여기에 SACK 정보 저장
```

---

## SACK Permitted vs SACK Block

| 구분 | 설명 | 사용 시점 |
|------|------|-----------|
| SACK Permitted | "나는 SACK 기능 사용 가능" | 3-Way Handshake 시 협상 |
| SACK Block | 실제 수신 완료 구간 표시 | 데이터 전송 중 |

SACK Block 예시:
```
SACK Block: 4001 ~ 6001
→ Sequence Number 4001~6000 이미 수신 완료
```

---

## Duplicate ACK와 SACK 관계

```
ACK=3
ACK=3
ACK=3  ← Duplicate ACK 3회 → Fast Retransmit 발동

+ SACK=4~5 함께 전달 시
→ 송신자: "3만 보내면 된다" 정확히 파악
```

---

## 자주 틀리는 부분

❌ SACK는 기본 TCP Header에 있다 → Options 필드에 있음  
❌ SACK는 항상 활성화되어 있다 → 3-Way Handshake 시 협상 필요  
❌ SACK Block의 Right Edge가 수신 완료 마지막 번호 → Right Edge는 마지막+1 번호
