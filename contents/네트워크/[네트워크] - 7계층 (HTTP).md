# [네트워크] - 7계층 (HTTP)

### -HTTP 프로토콜

HyperText Transfer Protocol (하이퍼 텍스트 전송 프로토콜)

www에서 쓰이는 핵심 프로토콜로 문서의 전송을 위해 쓰이며, 오늘날 거의 모든 웹 애플리케이션에서 사용되고 있다.

- 음성, 화상 등 여러 종류의 데이터를 MIME로 정의하여 전송 가능

- Request / Response (요청/응답) 동작에 기반하여 서비스 제공

<br>

### - HTTP 프로토콜의 특징

#### HTTP 1.0의 특징

![image-20230131115338880](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131115338880.png)

- 연결 수립, 동작, 연결 해제 의 단순함이 특징, 하나의 URL은 하나의 TCP 연결
- HTML 문서를 전송 받은 뒤 연결을 끊고 다시 연결하여 데이터를 전송한다.

#### HTTP 1.0의 문제점

- 단순 동작(연결 수립, 동작, 연결 해제)이 반복되어 통신 부하 문제 발생

<BR>

#### HTTP 1.1의 특징

![image-20230131115414361](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131115414361.png)

- HTTP 1.0과 호환 가능
- Multiple Request 처리가 가능하여 Client의 Request가 많을 경우 연속적인 응답 제공
  - Pipeline 방식의 Request / Response 진행
- HTTP 1.0과는 달리 Server가 갖는 하나의 IP Address와 다수의 Web Site 연겨 ㄹ가능

- 빠른 속도와 Internet Protocol 설계에 최적화될 수 있도록 Cache 사용 Data를 압축해서 전달이 가능하도록 하여 전달하는 Data 양이 감소

<br>

### - HTTP 요청 프로토콜

![image-20230131115839109](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131115839109.png)

##### HTTP 요청 프로토콜 예시

![ ](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131115958117.png)

![image-20230131120102108](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131120102108.png)

<BR>

### - HTTP 메소드 요청 방식

![image-20230131120150423](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131120150423.png)

<BR>

### - URI의 구조

![image-20230131120930201](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230131120930201.png)

<br>

### - HTTP 응답 프로토콜의 구조

![image-20230201122923293](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201122923293.png)

![image-20230201123028950](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201123028950.png)

![image-20230201123042767](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201123042767.png)

   

<BR>

### - HTTP 헤더

![image-20230201123615916](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201123615916.png)

![image-20230201123632037](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201123632037.png)

![image-20230201123658382](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201123658382.png)

![image-20230201123744793](%5B%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%5D%20-%207%EA%B3%84%EC%B8%B5%20(HTTP).assets/image-20230201123744793.png)
