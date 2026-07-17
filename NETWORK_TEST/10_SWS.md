# SWS (Silly Window Syndrome)

> ⭐ Clark Solution = 수신 측 / Nagle Algorithm = 송신 측

---

## SWS란?

매우 작은 크기의 데이터를 반복적으로 전송하여 네트워크 효율이 크게 떨어지는 현상.

```
TCP Header : 20Byte
Payload    : 1Byte
→ 전체 21Byte 중 데이터는 1Byte뿐 → 오버헤드 95%
```

---

## 발생 원인

- 작은 Window
- 작은 Payload
- 잦은 ACK
- Application이 Receive Buffer를 조금씩만 소비

송신 측과 수신 측 **모두에서** 발생 가능.

---

## 송신 측에서 발생하는 경우

Window가 10Byte라면 10Byte씩 계속 전송 → 작은 Segment 반복 생성

---

## 수신 측에서 발생하는 경우

Buffer 1000Byte인데 Application이 5Byte씩만 읽음
→ Window = 5 계속 광고
→ 송신자 5Byte씩 계속 전송 → SWS 발생

---

## 문제점

- 패킷 수 증가
- Header 오버헤드 증가
- CPU 사용량 증가
- 네트워크 대역폭 낭비
- 전체 처리량(Throughput) 감소

---

## 해결 방법 비교

| 구분 | Clark Solution | Nagle Algorithm |
|------|----------------|-----------------|
| 적용 위치 | **수신 측** | **송신 측** |
| 목적 | 작은 Window 광고 방지 | 작은 Payload 전송 방지 |
| 동작 방식 | 충분한 공간 확보 후 Window 광고 | 데이터 모아서 전송 |
| 효과 | 작은 Window 감소 | 작은 Segment 감소 |

---

## Clark Solution 상세

버퍼가 조금 비었다고 바로 작은 Window를 광고하지 않고, 충분한 공간이 확보될 때까지 기다렸다가 한 번에 큰 Window를 광고.

```
Window = 5  → 광고하지 않음
Window = 500 → 광고
```

---

## Nagle Algorithm 상세

작은 데이터를 즉시 보내지 않고, 이전 데이터 ACK가 오거나 충분한 데이터가 모이면 한 번에 전송.

```
1B + 2B + 3B + 4B → 10B 한 번에 전송
```

**Nagle Algorithm 비활성화 (TCP_NODELAY) 사례:**
- SSH (키 입력 응답성 중요)
- 온라인 게임 (지연 최소화)
- VoIP (실시간성 중요)

---

## 암기법

```
Clark → 수신 측 (Window 광고 조절)
Nagle → 송신 측 (Payload 모아서 전송)
```

---

## 자주 틀리는 부분

❌ Clark Solution은 송신 측 해결법 → 수신 측  
❌ Nagle Algorithm은 수신 측 해결법 → 송신 측  
❌ Nagle Algorithm은 항상 활성화해야 한다 → 지연에 민감한 앱에서는 비활성화
