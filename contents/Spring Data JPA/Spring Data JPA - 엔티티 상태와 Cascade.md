# Spring Data JPA - Cascade

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>
<br>

Cascade : 엔티티의 상태 변화를 전파 시키는 옵션

![image-20230216114524655](Spring%20Data%20JPA%20-%20%EC%97%94%ED%8B%B0%ED%8B%B0%20%EC%83%81%ED%83%9C%EC%99%80%20Cascade.assets/image-20230216114524655.png)

## 엔티티의 상태란? 

#### Transient

- JPA가 모르는 상태, DB에 들어갈지 안들어갈지 모르는 상태

<BR>

#### Persistent

- JPA가 관리중인 상태 (1차 캐시, Dirty Checking, Write Behind, ...) 
  - 객체의 변경사항을 계속해서 모니터링하고 있다가 반영해주겠다!

- Persistent 상태가 되었다고, 바로 insert가 발생해서 db에 저장하는 것이 아니라, Persistent에서 관리하고 있던 객체가 db에 넣는 시점에 데이터를 저장한다.
  - `Session.save()`, `Session.get()`, ....

- 1차 캐시 : `Persistent Context` (EntityManager, Session)에 인스턴스를 넣은 것
  - 아직 저장이 되지 않은 상태에서 다시 인스턴스를 달라고 요청하면, 이미 객체가 있기 때문에 db에 가지 않고 캐시에 있는 것을 준다.
- Dirty Checking : 이 객체의 변경사항을 계속해서 감지한다.
- Write Behind : 객체의 상태 변화를 db에 최대한 늦게, 최대한 가장 필요한 시점의 반영한다는 개념

<BR>

#### Detached

- JPA가 더이상 관리하지 않는 상태. 
- 트랜잭션이 끝났을 때 이미 db에 저장이 되고 `Session`이 끝난 상태

<br>

#### Removed

- JPA가 관리하긴 하지만 삭제하기로 한 상태.
- `Session.delete()`
