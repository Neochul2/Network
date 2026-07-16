# TCP/IP Advanced

# Chapter 10

# Flow Control

------------------------------------------------------------------------

# 학습 목표

이번 장에서는 TCP의 **흐름 제어(Flow Control)** 를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

-   Flow Control이 필요한 이유
-   Receiver Buffer의 역할
-   Window Size 조절
-   Zero Window
-   Window Update
-   Sliding Window와 Flow Control의 관계

------------------------------------------------------------------------

# 1. Flow Control이란?

Flow Control은 **수신자가 처리할 수 있는 속도에 맞추어 송신자의 전송
속도를 조절하는 기능**이다.

> 상대방이 받을 수 있는 만큼만 보내는 기술이다.

------------------------------------------------------------------------

# 2. 왜 필요한가?

예를 들어

``` text
송신자 : 100MB/s

↓

수신자 : 10MB/s
```

수신자는 데이터를 모두 처리하지 못해 **Receive Buffer**가 가득 차게
된다.

TCP는 이를 방지하기 위해 Flow Control을 수행한다.

------------------------------------------------------------------------

# 3. Receiver Buffer

수신자는 데이터를 즉시 애플리케이션에 전달하지 않고 먼저 버퍼에
저장한다.

``` text
TCP

↓

Receive Buffer

↓

Application
```

Receive Buffer는 데이터를 임시 보관하는 메모리 공간이다.

------------------------------------------------------------------------

# 4. Window Size와 Buffer의 관계

TCP Header의 **Window Size**는 수신자가 현재 추가로 받을 수 있는
공간(Byte)을 의미한다.

예)

``` text
Receive Buffer : 1000Byte

사용 중 : 400Byte

남은 공간 : 600Byte

↓

Window = 600
```

송신자는 Window 크기를 초과하여 전송하지 않는다.

------------------------------------------------------------------------

# 5. Zero Window

버퍼가 가득 차면

``` text
Window = 0
```

이 된다.

이를 **Zero Window**라고 한다.

Zero Window 상태에서는 송신자가 새로운 데이터를 보내지 않는다.

> 실제 TCP에서는 일정 시간마다 **Zero Window Probe**를 보내 Window가
> 열렸는지 확인한다.

------------------------------------------------------------------------

# 6. Window Update

애플리케이션이 버퍼의 데이터를 처리하면 여유 공간이 생긴다.

``` text
Window = 0

↓

Application 처리

↓

Window = 500
```

수신자는 새로운 Window Size를 광고하며 이를 **Window Update**라고 한다.

송신자는 Window Update를 받은 후 다시 데이터를 전송한다.

------------------------------------------------------------------------

# 7. Flow Control 동작 과정

``` text
데이터 전송
      │
      ▼
Receive Buffer 저장
      │
      ▼
Window 계산
      │
      ▼
Window Advertisement
      │
      ▼
송신자는 Window만큼 전송
      │
      ▼
Buffer Full?
      │
 ┌────┴────┐
 ▼         ▼
No        Yes
 │          │
 ▼          ▼
계속전송  Zero Window
              │
              ▼
      Window Update
              │
              ▼
          전송 재개
```

------------------------------------------------------------------------

# 8. Sliding Window와의 관계

Sliding Window는 **Flow Control을 구현하는 메커니즘**이다.

``` text
Flow Control

↓

Sliding Window

↓

Window Size 조절
```

------------------------------------------------------------------------

# 9. 교수님 수업 포인트

-   Window Size는 Receive Buffer의 남은 공간이다.
-   Buffer가 가득 차면 Window는 감소한다.
-   Window가 0이면 송신자는 전송을 중지한다.
-   Window Update 후 전송을 재개한다.

------------------------------------------------------------------------

# 10. Wireshark에서는?

TCP Header의 Window Size 항목에서 확인할 수 있다.

예)

``` text
Window Size = 8192
```

또는

``` text
Window Size = 0
```

------------------------------------------------------------------------

# 11. 실무 예시

대용량 파일을 다운로드하는 동안 애플리케이션이 데이터를 천천히 읽으면
Receive Buffer가 점점 가득 찬다.

그러면 TCP는 Window Size를 줄이고 송신자는 전송 속도를 낮춘다.

버퍼가 비워지면 Window Size가 증가하고 전송이 다시 빨라진다.

------------------------------------------------------------------------

# 12. Flow Control과 Congestion Control

  Flow Control                Congestion Control
  --------------------------- ------------------------------
  수신자의 처리 능력을 보호   네트워크 혼잡을 방지
  Receive Buffer 기준         네트워크 상태 기준
  Window Advertisement 사용   Congestion Window(cwnd) 사용

------------------------------------------------------------------------

# 시험 포인트

-   Flow Control은 수신자의 처리 속도에 맞추어 송신 속도를 조절한다.
-   Window Size는 Receive Buffer의 남은 공간이다.
-   Window = 0이면 Zero Window이다.
-   Window Update 후 전송을 재개한다.
-   Sliding Window는 Flow Control을 구현하는 기술이다.

------------------------------------------------------------------------

# 암기법

``` text
Buffer 감소
    │
    ▼
Window 감소
    │
    ▼
전송 감소

Buffer 증가
    │
    ▼
Window 증가
    │
    ▼
전송 증가
```

------------------------------------------------------------------------

# 면접 질문

1.  Flow Control이란 무엇인가?
2.  Window Size는 무엇을 의미하는가?
3.  Zero Window란 무엇인가?
4.  Sliding Window와 Flow Control의 관계는?
5.  Flow Control과 Congestion Control의 차이는?

------------------------------------------------------------------------

# 핵심 요약

-   Flow Control은 수신자의 처리 능력을 보호하는 기술이다.
-   Window Size는 Receive Buffer의 남은 공간을 의미한다.
-   Window가 0이 되면 송신자는 전송을 중지한다.
-   Window Update를 통해 전송을 재개한다.
-   Sliding Window는 Flow Control을 구현하는 핵심 메커니즘이다.

------------------------------------------------------------------------

# 다음 장

**Chapter 11 - Silly Window Syndrome (SWS)**

-   SWS란?
-   발생 원인
-   성능 저하 이유
-   해결 방법
