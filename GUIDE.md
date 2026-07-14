# GUIDE.md

# TCP/IP 전송계층 완전정리
## 집필 가이드 (Professor Edition)

---

# 프로젝트 목표

이 프로젝트의 목표는

교수님의 TCP/IP 전송계층 PDF를 기반으로

단순 요약이 아닌

**하나의 교재(Book)**

를 만드는 것이다.

최종 결과물은

교수님 PDF를 다시 보지 않아도 될 정도의

완성도를 가진 Markdown 교재를 목표로 한다.

---

# 대상 독자

이 교재는

TCP/IP를 처음 배우는 대학생을 대상으로 한다.

독자가 알고 있다고 가정하는 내용

- 컴퓨터 기본 개념
- 인터넷 사용 경험
- OSI 7계층 기본 구조
- IP 주소 개념

독자가 모른다고 가정하는 내용

- TCP Header
- Sequence Number
- ACK
- Sliding Window
- Flow Control
- FSM
- TCP 내부 동작

새로운 용어는 반드시

기존 개념을 이해한 후 등장해야 한다.

---

# 교수님 PDF 활용 원칙

교수님 PDF는

이 교재의 기준이다.

반드시

- 내용 삭제 금지
- 내용 누락 금지
- 용어 왜곡 금지

하지만

설명 순서는

학생이 이해하기 쉽도록

재배치할 수 있다.

ASCII 그림

표

예제

실제 사례

보충 설명

추가는 가능하다.

---

# 집필 철학

TCP/IP는

프로토콜을 외우는 과목이 아니다.

문제를 해결하기 위해

TCP가 왜 만들어졌는지를

이해하는 것이 목표이다.

항상

왜?

↓

문제는?

↓

해결 방법은?

↓

동작 원리

↓

예제

↓

실제 사용

↓

시험

순으로 설명한다.

---

# 작성 원칙

1.
PDF 내용 삭제 금지

2.
PDF 내용 누락 금지

3.
학생이 이해하기 쉬운 순서로 재구성

4.
원본 내용을 왜곡하지 않는다.

5.
모든 장은 자연스럽게 연결된다.

6.
정의를 먼저 외우게 하지 않는다.

7.
항상

왜?

부터 시작한다.

8.
시험 포인트는

본문 마지막에만 작성한다.

9.
같은 용어는 끝까지 동일하게 사용한다.

10.
같은 내용을 반복하지 않는다.

---

# 설명 순서

모든 개념은

왜 필요한가?

↓

이전 기술의 문제

↓

무엇을 해결하는가?

↓

핵심 개념

↓

동작 원리

↓

ASCII 그림

↓

예제

↓

실제 사용 사례

↓

시험 포인트

↓

자주 틀리는 부분

↓

핵심 요약

순으로 작성한다.

---

# 용어 규칙

다음 용어는 끝까지 동일하게 사용한다.

- TCP
- UDP
- Port
- Segment
- Payload
- Sequence Number
- ACK Number
- Window
- Sliding Window
- Flow Control
- Congestion Control
- Virtual Header
- MSS

---

# ASCII 그림 규칙

모든 그림은 Markdown에서 깨지지 않는

ASCII만 사용한다.

예)

Application
      │
      ▼
Transport
      │
      ▼
IP
      │
      ▼
Ethernet

또는

Client                  Server

SYN -------------------->

      <----------------- SYN + ACK

ACK -------------------->

---

# 표 규칙

비교가 필요한 내용은

반드시 표를 사용한다.

예)

- TCP vs UDP
- TCP Header vs UDP Header
- Port 번호
- FSM 상태 비교

---

# 시험 포인트

본문에서는

설명만 한다.

시험 포인트는

각 장 마지막에만 작성한다.

포함 내용

- 시험 포인트
- 자주 틀리는 문제
- 핵심 요약

---

# 파일 구조

00_학습로드맵.md

01_왜_전송계층이_필요한가.md

02_TCP와_UDP.md

03_Port와_다중화.md

04_UDP.md

05_TCP.md

06_TCP_특성.md

07_TCP_스트림과_Sequence_Number.md

08_TCP_Header.md

09_TCP_신뢰성_ACK_PAR.md

10_Sliding_Window_및_Flow_Control.md

11_TCP_연결_3Way_4Way.md

12_TCP_FSM.md

13_시험_핵심정리.md

---

# 최종 목표

이 Markdown 하나만 읽으면

교수님 PDF를 다시 보지 않아도 될 정도의

완성도를 가진

TCP/IP 전송계층 교재를 만드는 것을 목표로 한다.