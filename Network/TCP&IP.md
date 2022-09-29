# TCP/IP (흐름제어/혼잡제어)

------

> TCP 통신이란, 네트워크 통신에서 신뢰적인 연결방식, 기본적으로 unreliable network에서, reliable network를 보장할 수 있도록 하는 프로토콜이며 network congestion avoidance algorithm을 사용한다.

<BR>

- #### reliable network를 보장한다는 것은 4가지 문제점 존재

  1. **손실** : packet이 손실될 수 있는 문제
  2. **순서 바뀜** : packet의 순서가 바뀌는 문제
  3. **Congestion** : 네트워크가 혼잡한 문제
  4. **Overload** : receiver가 overload 되는 문제

<BR>

- #### 흐름제어/혼잡제어란?

  - 흐름제어(endsystem 대 endsystem)

    - 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법

    - Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것

    - 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점

      

  - **혼잡제어** : 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법

  <BR>

- #### 전송의 전체 과정

  - Application layer : sender application layer가 socket에 data를 씀.
  - Transport layer : data를 segment에 감싼다. 그리고 network layer에 넘겨줌.
  - 그러면 아랫단에서 어쨋든 receiving node로 전송이 됨. 이 때, sender의 send buffer에 data를 저장하고, receiver는 receive buffer에 data를 저장함.
  - application에서 준비가 되면 이 buffer에 있는 것을 읽기 시작함.
  - 따라서 flow control의 핵심은 이 receiver buffer가 넘치지 않게 하는 것임.
  - 따라서 receiver는 RWND(Receive WiNDow) : receive buffer의 남은 공간을 홍보함

<br>

### 1. 흐름제어 (Flow Control)

- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제없지만, 송신측의 속도가 빠를 경우 문제가 생긴다.
- 수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번이 발생한다.
- 이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야한다.

- #### 해결방법

  - **Stop and Wait** : 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

  - **Sliding Window** (Go Back N ARQ)

    - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어기법

    - 목적 : 전송은 되었지만, acked를 받지 못한 byte의 숫자를 파악하기 위해 사용하는 protocol

      LastByteSent - LastByteAcked <= ReceivecWindowAdvertised

      (마지막에 보내진 바이트 - 마지막에 확인된 바이트 <= 남아있는 공간) ==

      (현재 공중에 떠있는 패킷 수 <= sliding window)

  - **동작방식** : 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송

  - **Window** : TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 '3 way handshaking'을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 된다.