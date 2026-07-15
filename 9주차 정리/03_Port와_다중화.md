# TCP/IP 네트워크

# PART 3. Port와 다중화(Multiplexing)

> **학습 목표**
>
> - Port의 개념을 이해한다.
> - IP 주소와 Port 번호의 차이를 설명할 수 있다.
> - Multiplexing과 Demultiplexing의 원리를 이해한다.
> - Well-Known Port를 구분할 수 있다.

---

# 왜 Port가 필요한가?

앞 장에서는

TCP와 UDP가

전송계층의 대표 프로토콜이라는 것을 배웠습니다.

하지만 아직 해결되지 않은 문제가 있습니다.

IP는

컴퓨터까지는 찾아갈 수 있습니다.

그러나

컴퓨터 안에는

여러 프로그램이 동시에 실행됩니다.

예를 들어

```text
Chrome

Discord

Steam

Zoom

Outlook
```

모두 동시에 인터넷을 사용할 수 있습니다.

그렇다면

도착한 데이터는

어느 프로그램으로 전달해야 할까요?

---

# IP 주소의 한계

IP 주소는

컴퓨터(Host)를 식별합니다.

예를 들어

```text
192.168.0.10
```

이라는 주소는

목적지 컴퓨터를 찾을 수 있습니다.

하지만

컴퓨터 안에 있는

Chrome인지

Discord인지

Steam인지

구분할 수 없습니다.

즉,

IP 주소는

컴퓨터까지만 찾아갑니다.

---

# Port란?

Port는

컴퓨터 안에서

실행 중인 프로그램(Process)을

식별하기 위한 번호입니다.

즉

```text
IP

↓

컴퓨터 찾기

↓

Port

↓

프로그램 찾기
```

라는 과정을 수행합니다.

---

# 실생활 예시

IP 주소를

아파트 주소라고 생각해 봅시다.

```text
서울특별시 ○○구 ○○로
```

여기까지는

IP 주소입니다.

하지만

실제로 택배를 전달하려면

```text
101동 1203호
```

가 필요합니다.

이 역할을 하는 것이

Port 번호입니다.

---

# Socket이란?

실제 통신에서는

IP 주소와 Port 번호를 함께 사용합니다.

이를

Socket Address

라고 합니다.

예를 들어

```text
192.168.0.10:80
```

또는

```text
203.0.113.10:443
```

처럼

IP 주소와 Port 번호를 함께 표현합니다.

---

# Port 번호의 범위

Port 번호는

16bit를 사용합니다.

따라서

사용 가능한 범위는

```text
0 ~ 65535
```

입니다.

교수님 PDF에서는

Port를 다음과 같이 구분합니다.

|구분|번호 범위|설명|
|---|---|---|
|Well-Known Port|0~1023|표준 서비스|
|Registered Port|1024~49151|등록된 프로그램|
|Dynamic(Private) Port|49152~65535|임시 Port|

---

# Well-Known Port

대표적인 Well-Known Port는 다음과 같습니다.

|Port|프로토콜|
|---:|---|
|20, 21|FTP|
|22|SSH|
|23|Telnet|
|25|SMTP|
|53|DNS|
|67|DHCP Server|
|68|DHCP Client|
|80|HTTP|
|110|POP3|
|143|IMAP|
|443|HTTPS|

이 번호들은

IANA(Internet Assigned Numbers Authority)가 관리합니다.

---

# Source Port와 Destination Port

TCP와 UDP에는

항상 두 개의 Port 번호가 존재합니다.

- Source Port
- Destination Port

예를 들어

웹 브라우저가

웹 서버에 접속하면

```text
Client

Source Port

52341

↓

Destination Port

80

↓

Web Server
```

처럼 사용됩니다.

응답 시에는

Source와 Destination이 서로 바뀝니다.

---

# Multiplexing(다중화)

여러 프로그램이

동시에 데이터를 보내는 과정을

Multiplexing이라고 합니다.

```text
Chrome

Discord

Steam

Zoom

      │

      ▼

TCP / UDP

      │

      ▼

IP
```

여러 프로그램의 데이터를

하나의 전송계층으로 모으는 과정입니다.

---

# Demultiplexing(역다중화)

수신 측에서는

Port 번호를 확인하여

올바른 프로그램으로 전달합니다.

```text
IP

↓

TCP / UDP

↓

Port 확인

↓

Chrome

Discord

Steam

Zoom
```

이 과정을

Demultiplexing이라고 합니다.

---

# 실제 통신 예

웹 브라우저가

웹 서버에 접속하는 경우

```text
Client

192.168.0.10:52341

──────────────▶

Server

203.0.113.10:80
```

응답

```text
Server

203.0.113.10:80

──────────────▶

Client

192.168.0.10:52341
```

Port 번호를 이용하여

올바른 프로그램으로

데이터가 전달됩니다.

---

# Port가 없다면?

Port 번호가 없다면

웹 브라우저가 요청한 데이터가

게임 프로그램으로 전달될 수도 있습니다.

메일 데이터가

동영상 플레이어로 전달될 수도 있습니다.

즉,

프로그램 간 통신이 불가능해집니다.

---

# 왜 다음 장이 필요한가?

Port 번호를 이용하면

프로그램까지는

정확하게 전달할 수 있습니다.

하지만

UDP는

연결을 만들지 않고

재전송도 하지 않습니다.

그렇다면

UDP는

어떻게 동작할까요?

다음 장에서는

UDP의 구조와

UDP Header를 자세히 학습합니다.

---

# 시험 포인트

- IP 주소는 Host를 식별한다.
- Port 번호는 Process를 식별한다.
- Port 번호는 16bit이다.
- Well-Known Port는 0~1023이다.
- Multiplexing은 여러 프로그램의 데이터를 하나로 모으는 과정이다.
- Demultiplexing은 Port 번호를 이용하여 프로그램으로 전달하는 과정이다.

---

# 자주 틀리는 부분

❌ IP 주소가 프로그램을 구분한다.

→ 아니다.

프로그램을 구분하는 것은 Port 번호이다.

---

❌ 서버와 클라이언트는 항상 같은 Port를 사용한다.

→ 아니다.

일반적으로 서버는 Well-Known Port를 사용하고,

클라이언트는 Dynamic Port를 사용한다.

---

# 핵심 요약

- IP는 컴퓨터를 찾는다.
- Port는 프로그램을 찾는다.
- Socket은 IP 주소와 Port 번호의 조합이다.
- Port 번호는 TCP와 UDP 모두에서 사용된다.
- Multiplexing과 Demultiplexing은 전송계층의 핵심 기능이다.

---

# 다음 장

**PART 4. UDP(User Datagram Protocol)**