# TCP/IP 네트워크

# PART 8. TCP Header

> **학습 목표**
>
> - TCP Header의 구조를 이해한다.
> - 각 Header 필드의 역할을 설명할 수 있다.
> - Sequence Number와 ACK Number가 Header에서 어떻게 사용되는지 이해한다.
> - TCP Header가 신뢰성 있는 통신을 어떻게 지원하는지 이해한다.

---

# 왜 TCP Header를 배우는가?

앞 장에서는

TCP가 Byte Stream을 관리하며

각 Byte에

Sequence Number를 부여한다는 것을 배웠습니다.

그렇다면

이러한 정보들은

어디에 저장될까요?

정답은

TCP Header입니다.

TCP는

데이터를 전송할 때

Payload 앞에

TCP Header를 추가합니다.

이 Header에는

통신에 필요한 모든 제어 정보가 저장됩니다.

---

# TCP Segment의 구조

TCP가 전송하는 데이터는

다음과 같은 구조를 가집니다.

```text
+----------------------+
|      TCP Header      |
+----------------------+
|       Payload        |
+----------------------+
```

- **TCP Header** : 통신을 제어하는 정보
- **Payload** : 실제 전송할 데이터

---

# TCP Header의 크기

TCP Header의 기본 크기는

```text
20 Byte
```

입니다.

옵션(Option)을 사용하는 경우

최대

```text
60 Byte
```

까지 확장될 수 있습니다.

---

# TCP Header 구조

```text
0                   15 16                  31

+----------------------+----------------------+
|     Source Port      |   Destination Port   |
+----------------------+----------------------+
|              Sequence Number               |
+--------------------------------------------+
|           Acknowledgment Number            |
+----+------+----------------+---------------+
|HLEN|Reserved| Control Bits | Window Size   |
+-----------------------------+--------------+
|        Checksum             | Urgent Ptr   |
+-----------------------------+--------------+
|        Options (Optional)                  |
+--------------------------------------------+
|                Payload                     |
+--------------------------------------------+
```

---

# 1. Source Port

송신 프로그램의 Port 번호입니다.

예를 들어

웹 브라우저는

임시 Port를 사용합니다.

```text
52341
```

---

# 2. Destination Port

수신 프로그램의 Port 번호입니다.

예를 들어

웹 서버는

```text
80
```

HTTPS 서버는

```text
443
```

을 사용합니다.

---

# 3. Sequence Number

TCP에서

가장 중요한 필드입니다.

현재 Segment의

첫 번째 Byte 번호를 저장합니다.

예를 들어

```text
HELLO
```

가

101번 Byte부터 시작한다면

```text
Sequence Number = 101
```

이 저장됩니다.

수신자는

이 번호를 이용하여

데이터의 순서를 맞춥니다.

---

# 4. Acknowledgment Number

상대방에게

다음에 받아야 할 Byte 번호를 알려줍니다.

예를 들어

100Byte를 정상적으로 받았다면

```text
ACK Number = 101
```

입니다.

즉

101번 Byte부터

계속 보내라는 의미입니다.

---

# 5. Header Length(Data Offset)

TCP Header의 길이를 나타냅니다.

기본 Header는

20Byte입니다.

Option을 사용할 경우

Header Length 값이 증가합니다.

---

# 6. Reserved

예약 영역입니다.

현재는

사용하지 않습니다.

향후 기능 확장을 위해

남겨둔 영역입니다.

---

# 7. Control Bits(Flags)

TCP 연결과

통신 상태를 제어하는 비트입니다.

대표적인 Control Bit는 다음과 같습니다.

|Flag|역할|
|---|---|
|SYN|연결 생성|
|ACK|수신 확인|
|FIN|정상 종료|
|RST|비정상 종료|
|PSH|즉시 전달 요청|
|URG|긴급 데이터 표시|

이 비트들은

TCP 연결 과정에서

매우 중요한 역할을 수행합니다.

---

# 8. Window Size

수신자가

현재 받을 수 있는 데이터의 크기를 나타냅니다.

이 값을 이용하여

송신자는

너무 많은 데이터를 보내지 않습니다.

Flow Control에서

사용되는 핵심 필드입니다.

---

# 9. Checksum

데이터가

전송 중 손상되었는지 검사합니다.

송신자와 수신자가

같은 계산 결과를 얻으면

정상 데이터입니다.

결과가 다르면

오류가 발생한 것입니다.

---

# TCP Virtual Header

TCP도

Checksum 계산 시

Virtual Header를 사용합니다.

Virtual Header에는

- Source IP
- Destination IP
- Protocol 번호
- TCP Length

정보가 포함됩니다.

중요한 점은

Virtual Header는

Checksum 계산에만 사용되며

실제 네트워크로 전송되지는 않습니다.

---

# 10. Urgent Pointer

URG 비트가 설정된 경우

긴급 데이터의 위치를 나타냅니다.

현재 대부분의 응용프로그램에서는

거의 사용되지 않습니다.

---

# 11. Options

TCP의 추가 기능을 저장합니다.

대표적인 Option은

- MSS(Maximum Segment Size)
- Window Scale
- SACK
- Timestamp

등이 있습니다.

연결을 생성하는 과정에서

주로 사용됩니다.

---

# TCP Header가 중요한 이유

TCP Header에는

통신에 필요한

모든 제어 정보가 저장됩니다.

예를 들어

- 연결 생성
- 연결 종료
- 데이터 순서
- 재전송
- ACK
- 흐름 제어

모두 Header의 정보를 이용하여 수행됩니다.

즉

TCP의 신뢰성은

TCP Header에서 시작됩니다.

---

# Header 필드 요약

|필드|역할|
|---|---|
|Source Port|송신 프로그램|
|Destination Port|수신 프로그램|
|Sequence Number|현재 Byte 번호|
|ACK Number|다음 받을 Byte 번호|
|Header Length|Header 크기|
|Reserved|예약 영역|
|Control Bits|연결 및 제어|
|Window Size|Flow Control|
|Checksum|오류 검출|
|Urgent Pointer|긴급 데이터|
|Options|추가 기능|

---

# 왜 다음 장이 필요한가?

TCP Header 안에는

Sequence Number와

ACK Number가 저장되어 있습니다.

그렇다면

이 두 값은

실제로 어떻게 사용되어

신뢰성을 제공할까요?

다음 장에서는

ACK,

Timeout,

Retransmission,

PAR를 중심으로

TCP의 신뢰성 메커니즘을 학습합니다.

---

# 시험 포인트

- TCP Header의 기본 크기는 20Byte이다.
- Sequence Number는 현재 Segment의 첫 번째 Byte 번호이다.
- ACK Number는 다음에 받을 Byte 번호이다.
- Window Size는 Flow Control에 사용된다.
- Checksum은 오류를 검출한다.
- Virtual Header는 Checksum 계산에만 사용되며 실제 전송되지 않는다.

---

# 자주 틀리는 부분

❌ ACK Number는 마지막으로 받은 Byte 번호이다.

→ 아니다.

다음에 받아야 할 Byte 번호이다.

---

❌ TCP Header는 항상 20Byte이다.

→ 아니다.

Option을 사용하면 최대 60Byte까지 확장될 수 있다.

---

# 핵심 요약

- TCP Header는 통신에 필요한 모든 제어 정보를 저장한다.
- Sequence Number와 ACK Number는 신뢰성 있는 통신의 핵심이다.
- Control Bit는 연결 생성과 종료를 제어한다.
- Window Size는 흐름 제어를 담당한다.
- Checksum은 데이터 오류를 검출한다.

---

# 다음 장

**PART 9. TCP 신뢰성(ACK, Timeout, Retransmission, PAR)**