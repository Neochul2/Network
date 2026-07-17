# Flow Control (흐름 제어)

> ⭐ Window Size = Receive Buffer 남은 공간 / Zero Window → 전송 중지 → Window Update → 재개

---

## Flow Control이란?

수신자가 처리할 수 있는 속도에 맞추어 송신자의 전송 속도를 조절하는 기능.

```
송신자: 100MB/s  →  수신자: 10MB/s
→ Receive Buffer 가득 참 → 데이터 손실
→ Flow Control로 방지!
```

---

## Receiver Buffer

수신자는 데이터를 즉시 Application에 전달하지 않고 Buffer에 먼저 저장.

```
TCP → Receive Buffer → Application
```

---

## Window Size와 Buffer 관계

```
Receive Buffer : 1000Byte
사용 중        :  400Byte
남은 공간      :  600Byte
→ Window = 600
```

송신자는 Window 크기를 초과하여 전송하지 않는다.

**Buffer 감소 → Window 감소 → 전송 감소**
**Buffer 증가 → Window 증가 → 전송 증가**

---

## Zero Window

Buffer가 가득 차면 Window = 0.

```
Window = 0 → 송신자 전송 중지
```

실제 TCP에서는 일정 시간마다 **Zero Window Probe**를 보내 Window가 열렸는지 확인.

---

## Window Update

Application이 Buffer 데이터를 처리하면 여유 공간 생성.

```
Window = 0 → Application 처리 → Window = 500
```

수신자가 새로운 Window Size를 광고 → 송신자 전송 재개.

---

## 송신 윈도우 크기 조절 예제

```
Client → Server 140B 전송
Server → ACK141 + Window=260 응답  ← Window 100 줄임
Client → SND.WND 260으로 조절
Client → 180B 전송
Server → ACK321 + Window=80 응답  ← Window 180 줄임
Client → SND.WND 80으로 조절
Client → 80B 전송
Server → ACK401 + Window=0  ← Zero Window
Client → 전송 중지
→ Server Window Update 후 재개
```

---

## Flow Control 동작 흐름

```
데이터 전송
    ↓
Receive Buffer 저장
    ↓
Window 계산
    ↓
Window Advertisement (TCP Header Window 필드)
    ↓
송신자는 Window만큼만 전송
    ↓
Buffer Full? → Yes → Zero Window → Window Update → 재개
            → No  → 계속 전송
```

---

## Flow Control vs Congestion Control

| 구분 | Flow Control | Congestion Control |
|------|-------------|-------------------|
| 목적 | 수신자 보호 | 네트워크 보호 |
| 기준 | Receive Buffer | 네트워크 상태 |
| 사용 | Window Advertisement | Congestion Window(cwnd) |

---

## 자주 틀리는 부분

❌ Sliding Window와 Flow Control은 다른 개념 → Sliding Window가 Flow Control을 구현하는 메커니즘  
❌ Flow Control = Congestion Control → 목적과 기준이 다름  
❌ Zero Window가 되면 연결이 끊긴다 → Zero Window Probe로 계속 확인
