# 🌱 Redis

[참고](https://gyoogle.dev/blog/computer-science/data-base/Redis.html)

> **Key, Value** 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 데이터 베이스 관리 시스템 (DBMS)!
>
> 데이터베이스, 캐시, 메세지 브로커로 사용되며 인메모리 데이터 구조를 가진 저장소!

- 일반적으로 데이터베이스는 하드 디스크나 SSD에 저장한다. 하지만 Redis는 메모리(RAM)에 저장해서 디스크 스캐닝이 필요없어 매우 빠른 장점을 가지고 있음

- 캐싱도 가능해서 실시간 채팅에 적합하며 세션 공유를 위해 세션 클러스터링에도 활용된다.
  - RAM은 껏다 키면 날아가는 휘발성을 가지고 있기 때문에 이를 막기위한 백업 과정이 존재한다.
  - snapshot : 특정 지점을 설정하고 디스크에 백업
  - AOF(Append Only File) : 명령(쿼리)들을 저장해두고, 서버가 셧다운되면 재실행해서 다시 만들어 놓는 것

데이터 구조는 `Key/Value` 값으로 이루어져 있다. (따라서 Redis는 비정형 데이터를 저장하는 비관계형 데이터베이스 관리 시스템이다.)

#### value 5가지

1. String (text, binary data) - 512MB까지 저장이 가능함
2. set (String 집합)
3. sorted set (set을 정렬해둔 상태)
4. Hash
5. List (양방향 연결리스트도 가능)