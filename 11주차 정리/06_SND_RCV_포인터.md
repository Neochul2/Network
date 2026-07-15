# TCP/IP Advanced

# Chapter 06
# SND / RCV 포인터

---

# 학습 목표

이번 장에서는 TCP가 Sliding Window를 구현하기 위해 사용하는
SND(Sender)와 RCV(Receiver) 포인터를 학습한다.

학습 후에는 다음 내용을 설명할 수 있어야 한다.

- SND.UNA
- SND.NXT
- SND.WND
- RCV.NXT
- RCV.WND
- 포인터가 어떻게 Sliding Window를 구현하는가

---

# 왜 포인터가 필요한가?

TCP는 Byte Stream 기반 프로토콜이다.

데이터를 계속 보내고 받으면서

- 어디까지 보냈는가?
- 어디까지 ACK를 받았는가?
- 다음에는 무엇을 보내야 하는가?
- 상대방은 얼마나 더 받을 수 있는가?

를 계속 기억해야 한다.

이 정보를 관리하기 위해 사용하는 것이

**TCP Pointer**이다.

---

# 포인터의 종류

TCP는 크게

송신(Sender)

수신(Receiver)

두 그룹의 포인터를 사용한다.

```text
송신

SND.UNA

SND.NXT

SND.WND

↓

수신

RCV.NXT

RCV.WND
```

---

# SND.UNA

(Sender UnAcknowledged)

의미

> 아직 승인받지 못한 가장 오래된 Byte의 Sequence Number

예)

```text
1~100

전송 완료

ACK 미도착
```

↓

```
SND.UNA = 1
```

ACK가

101

도착하면

```
SND.UNA

↓

101
```

로 이동한다.

---

# 쉽게 이해하기

택배를

10개 보냈다고 생각하자.

택배

1

2

3

4

5

...

아직

1번에 대한

배송 완료 문자가 안 왔다.

그러면

가장 오래 기다리는 택배는

1번이다.

그것이

SND.UNA이다.

---

# SND.NXT

(Sender Next)

의미

> 다음에 보낼 Byte의 Sequence Number

예)

```text
1~100

전송
```

↓

다음

```
101
```

부터

보내야 한다.

따라서

```
SND.NXT

=

101
```

---

# SND.UNA와 SND.NXT 차이

```text
SND.UNA

↓

가장 오래된 미승인 데이터

------------------------

SND.NXT

↓

다음에 보낼 데이터
```

즉

둘 사이에는

이미 전송했지만

ACK를 기다리는 데이터가 있다.

---

# SND.WND

(Sender Window)

송신 Window의 크기이다.

즉

ACK를 받지 않아도

추가로 보낼 수 있는

최대 Byte 수이다.

예)

```
SND.WND

=

500
```

이라면

500Byte까지

추가 전송 가능하다.

---

# RCV.NXT

(Receiver Next)

수신자가

다음에 받고 싶은 Byte 번호이다.

예)

```
1~100

수신 완료
```

↓

다음

```
101
```

을

기다린다.

따라서

```
RCV.NXT

=

101
```

---

# ACK와 RCV.NXT

ACK 번호는

바로

RCV.NXT를

상대에게 알려주는 것이다.

즉

```text
ACK = 101

↓

101부터 보내세요.
```

라는 의미이다.

---

# RCV.WND

Receiver Window

수신 버퍼의

남은 크기이다.

예)

버퍼

1000Byte

현재

600Byte 사용

↓

남은 공간

400Byte

↓

```
RCV.WND

=

400
```

송신자는

이 값을 보고

얼마나 더 보낼 수 있는지

결정한다.

---

# 포인터 관계

```text
SND.UNA

↓

미승인 시작

---------------------

SND.NXT

↓

다음 전송

---------------------

SND.UNA + SND.WND

↓

Window 끝
```

Window 안의 데이터만

전송할 수 있다.

---

# 그림으로 이해하기

```text
| 완료 | ACK대기 | 전송가능 | 전송불가 |

      ^
   SND.UNA

             ^
          SND.NXT

---------------------------

Window 끝

SND.UNA + SND.WND
```

---

# ACK가 오면?

예)

ACK

101

도착

↓

```
SND.UNA

1

↓

101
```

Window도

같이

앞으로 이동한다.

이것이

Sliding Window이다.

---

# 교수님 수업 포인트

교수님께서는

다음 내용을 반복해서 설명하셨다.

★★★★★

SND.UNA

↓

가장 오래 기다리는 ACK

SND.NXT

↓

다음에 보낼 Byte

SND.WND

↓

Window 크기

RCV.NXT

↓

다음에 받을 Byte

RCV.WND

↓

남은 Buffer

이 다섯 개는 반드시 암기해야 한다.

---

# Wireshark에서는?

Wireshark에서는

직접

SND.UNA

같은 이름은

보이지 않는다.

하지만

다음 값을 통해

유추할 수 있다.

- Sequence Number
- ACK Number
- Window Size

이 세 값만 보면

포인터 이동을

분석할 수 있다.

---

# 실무 예시

파일을 다운로드한다고 가정하자.

```
1~1000Byte

전송
```

ACK

```
501
```

도착

↓

앞의

500Byte는

정상 수신

↓

Window 이동

↓

501부터

계속 전송

이 과정을

수천 번 반복한다.

---

# 시험 포인트

★★★★★

✔ SND.UNA

→ 가장 오래된 미승인 Byte

✔ SND.NXT

→ 다음 전송 Byte

✔ SND.WND

→ 송신 Window 크기

✔ RCV.NXT

→ 다음 수신 Byte

✔ RCV.WND

→ 수신 Buffer 크기

---

# 암기법

```text
UNA

↓

ACK 못 받은 것

-----------------

NXT

↓

Next

다음

-----------------

WND

↓

Window

크기
```

---

# 면접 질문

Q.

SND.UNA란 무엇인가?

Q.

SND.NXT와 차이는?

Q.

ACK 번호와 RCV.NXT의 관계는?

Q.

RCV.WND는 무엇을 의미하는가?

Q.

Sliding Window에서 포인터는 왜 필요한가?

---

# 핵심 요약

- TCP는 포인터를 이용하여 Byte Stream을 관리한다.
- SND.UNA는 가장 오래된 미승인 데이터를 가리킨다.
- SND.NXT는 다음에 보낼 데이터를 가리킨다.
- SND.WND는 송신 가능한 범위를 결정한다.
- RCV.NXT는 다음에 받을 데이터를 가리킨다.
- RCV.WND는 수신 가능한 버퍼 크기를 의미한다.

---

# 다음 장

Chapter 07에서는

교수님 PDF의 예제를 그대로 사용하여

140Byte 요청

↓

80Byte 응답

↓

120Byte 전송

과정을

SND와 RCV 포인터가 실제로 어떻게 이동하는지

한 단계씩 분석한다.