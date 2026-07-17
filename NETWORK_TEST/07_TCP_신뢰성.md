# TCP 신뢰성 (ACK, PAR, 재전송)

> ⭐ PAR = Positive Acknowledgment with Retransmission / Duplicate ACK 3회 → Fast Retransmit

---

## 신뢰성이란?

송신한 데이터가 손실 없이, 순서대로, 중복 없이 수신자에게 전달되는 것.

---

## PAR (Positive Acknowledgment with Retransmission)

TCP 신뢰성의 핵심 방식.

**기본 PAR:**
```
전송 → ACK 기다림 → ACK 도착 → 다음 데이터
                 → ACK 없음 → Timeout → 재전송
```

단점: 승인 받기 전까지 다른 메시지 전송 불가 → 비효율

**PAR 개선 (슬라이딩 윈도우 적용):**
- 메시지 ID 필드 추가
- 송신 메시지 제한 필드 추가
- 여러 메시지 동시 전송 가능

---

## ACK (Acknowledgment)

수신자가 데이터를 받았다는 확인 메시지.

```
1~100 Byte 수신 완료
→ ACK = 101  (다음에 받을 Byte 번호)
```

**누적 ACK (Cumulative ACK):** 하나의 ACK가 여러 Segment를 동시에 승인 가능.

```
1~100, 101~200, 201~300 모두 수신
→ ACK = 301
```

---

## Retransmission Queue (재전송 큐)

ACK를 받지 못한 TCP Segment를 관리하는 송신 측 대기 영역.

```
전송 → Queue에 저장 → Timer 시작 → ACK 도착 → Queue에서 제거
                                 → Timeout → 재전송 → Timer 재시작
```

ACK 수신 예시:
```
ACK = 201 수신
→ Part1 삭제
→ Part2 삭제
→ Part3, Part4는 Queue에 남음
```

---

## RTT와 RTO

- **RTT (Round Trip Time):** Segment 전송 후 ACK 도착까지의 시간
- **RTO (Retransmission Timeout):** 재전송 타이머 값

TCP는 단순 평균이 아니라 **SRTT(Smoothed RTT)** + **RTTVAR(RTT Variance)** 로 RTO를 동적으로 계산한다. (RFC 6298)

```
Segment 전송 → RTT 측정 → SRTT/RTTVAR 갱신 → RTO 계산
```

네트워크 지연 증가 → RTO 증가 / 네트워크 지연 감소 → RTO 감소

---

## Fast Retransmit (빠른 재전송)

Timeout을 기다리지 않고 **Duplicate ACK 3회 수신 시** 즉시 재전송.

```
ACK100
ACK100
ACK100   ← 3회 반복
→ 즉시 재전송!
```

---

## 재전송 정책 2가지

**정책 ①: 타이머 만료된 Segment만 재전송**
- 장점: 불필요한 재전송 감소, 대역폭 절약
- 단점: 복구 속도 느림

**정책 ②: 미승인된 모든 Segment 재전송**
- 장점: 손실 복구 빠름
- 단점: 불필요한 재전송 증가, 대역폭 낭비

현대 TCP는 Fast Retransmit + Fast Recovery + SACK 조합으로 효율적 복구.

---

## ISN (Initial Sequence Number)

각 TCP 장비는 연결 초기화 시 **32비트 초기 순서 번호(ISN)** 를 선택한다.

3-Way Handshake 예시:
```
① SYN : 순서번호 = 4,567
② SYN+ACK : 승인번호 = 4,568 / 순서번호 = 12,998
③ ACK : 승인번호 = 12,999
```

- 송신자와 수신자는 스트림의 Byte에 할당할 순서 번호에 동의해야 함 → **동기화**
- 동기화는 TCP 연결이 수립될 때 수행됨

---

## TCB (Transmission Control Block)

각 TCP 연결의 데이터를 저장하는 특별한 데이터 구조.

저장 정보:
- 연결 식별을 위한 두 소켓 번호
- 들어온 데이터와 나갈 데이터를 가지고 있는 버퍼 포인터
- SND.UNA, SND.NXT, SND.WND, RCV.NXT, RCV.WND 등 포인터 값

각 연결이 독립적이기 때문에 연결마다 TCB를 생성하여 관리.

---

## 자주 틀리는 부분

❌ ACK는 마지막으로 받은 번호 → 다음에 받아야 할 번호  
❌ TCP는 항상 Timeout 후에만 재전송 → Duplicate ACK 3회 시 Fast Retransmit  
❌ PAR는 별도 프로토콜 → TCP가 사용하는 신뢰성 전송 방식  
❌ RTT = RTO → RTT는 측정값, RTO는 계산된 타이머 값
