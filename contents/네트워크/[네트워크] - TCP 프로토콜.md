# [네트워크] - TCP 프로토콜

전송 제어 프로토콜(TCP)은 인터넷에 연결된 컴퓨터에서 실행되는 프로그램 간의 통신을 **<u>안정적으로, 순선대로, 에러없이</u>** 교환할 수 있게 한다.

TCP의 안전성을 필요로 하지 않는 애플리케이션의 경우 일반적으로 TCP 대신 UDP를 사용한다.

TCP는 UDP보다 안전하지만 느리다.

<BR>

### - TCP 프로토콜의 구조

![image-20230127160515165](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%20TCP%20%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C.assets/image-20230127160515165.png)

- 가장 일반적인 길이는 20바이트이지만, 옵션 때문에 최대 60바이트까지 늘어날 수 있다. 

- Window : TCP는 연결을 지향하기 때문에, 데이터 보내도 되는지 확인, 잘 받았는지 확인, 얼만큼 더 보내 등등 전달할 때 사용한다



<BR>

### - TCP 플래그

![image-20230127162414004](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%20TCP%20%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C.assets/image-20230127162414004.png)

- TCP는 계속해서 통신하면서 상대방이랑 연결상태를 물어보는데 이 과정에서, 여러가지 형태로 데이터를 보낼 때 나타내는 값이 플래그

- U : 긴급 비트, 내가 지금 보내는 데이터가 우선순위가 높은 데이터가 포함되어 있다고 알려줌, U가 1일 때! 

- A : 승인 비트, TCP에서 굉장히 중요하고 많이 사용하는 플래그, 물어본 것에 대한 응답을 해줄 때, 응 지금 연결해도 돼! 
- P : 밀어넣기 비트, 그냥 데이터 계속해서 밀어넣겠다는 뜻
- R : 초기화 비트, 상대방이랑 연결되어 있는 상태에서 추가적으로 데이터를 주고받는 상황에 문제가 생길 때, 우리 둘 사이의 관계를 리셋하자는 뜻

- S : 동기화 비트, 상대방이랑 연결을 시작할 때 무조건 사용하는 플래그로서 얘가 처음 보내지고 난 다음부터 둘 사이가 동기화 되기 시작한다. 플래그 중 제일 중요
- F : 종료 비트, 데이터를 다 주고 받고나서 연결을 끊을 때 사용하는 플래그

![image-20230127163039952](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%20TCP%20%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C.assets/image-20230127163039952.png)

> U, A, P, R, S 를 주로 사용한다

<BR>

### - TCP를 이용한 통신과정

연결 수립 과정

TCP를 이용한 데이터 통신을 할 때 프로세스와 프로세스를 연결하기 위해 `가장 먼저 수행되는 과정`

1. 클라이언트가 서버에게 요청 패킷을 보내고
2. 서버가 클라이언트의 요청을 받아들이는 패킷을 보내고
3. 클라이언트는 이를 최종적으로 수락하는 패킷을 보낸다.

위의 3개의 과정을 3 Way Handshake라고 부른다.

 ![image-20230127165602971](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%20TCP%20%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C.assets/image-20230127165602971.png)

- 이렇게 플래그를 세팅해서 보낸다

<br>

데이터 송수신 과정

TCP를 이용한 데이터 통신을 할 때 단순히 TCP 패킷만을 캡슐화해서 통신하는 것이 아닌 페이로드를 포함한 패킷을 주고 받을 때의 일정한 규칙

1. 보낸 쪽에서 또 보낼 때는 SEQ번호와 ACK 번호가 그대로다.
2. 받는 쪽에서 SEQ 번호는 받은 ACK 번호가 된다.
3. 받는 쪽에서 ACK 번호는 받은 SEQ 번호 + 데이터의 크기