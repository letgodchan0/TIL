

# 1. 프로젝트 생성

![image-20220702101649539](springboot%20%EC%8B%9C%ED%96%89%EC%B0%A9%EC%98%A4.assets/image-20220702101649539.png)

<br>

## - 서버 실행하면 다음과 같은 에러 발생

![image-20220702102113230](springboot%20%EC%8B%9C%ED%96%89%EC%B0%A9%EC%98%A4.assets/image-20220702102113230.png)



#### application.properties 설정 추가

``` properties
#DataBase Setting
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url= jdbc:mysql://127.0.0.1:3306/ssafyweb?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8
spring.datasource.username=ssafy
spring.datasource.password=ssafy
```

- `spring.datasource.driver-class-name` : mysql 연동해 놓은 듯
- `spring.datasource.url` : 내 db 중 어떤 데이터베이스 쓸 것인지 여기서는 `ssafyweb` 데이터베이스 사용 뒤에는 utf 설정
- `spring.datasource.username` : mysql 관리자가 얘임

<br>

## 추가 설정

![image-20220702103058858](springboot%20%EC%8B%9C%ED%96%89%EC%B0%A9%EC%98%A4.assets/image-20220702103058858.png)

- [mapper-locations](https://codingdog.tistory.com/entry/mybatis-mapper-location-%EC%84%A4%EC%A0%95%EC%9D%84-%EC%89%BD%EA%B3%A0-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%ED%95%B4-%EB%B4%85%EC%8B%9C%EB%8B%A4) : mapper 폴더 어디있는지 알려줄려구

<br>

### 설정에 맞게 패키지랑 mapper 폴더 생성

1. com.ssafy.guestbook.model 패키지 생성
2. mapper 폴더 생성

<br>



# - member

### 1. Model 패키지 생성

![image-20220702104457430](springboot%20%EC%8B%9C%ED%96%89%EC%B0%A9%EC%98%A4.assets/image-20220702104457430.png)

- MemberDto 생성 -> NEW -> class에서 생성



### 2. MemberDto 설정

```java
package com.ssafy.guestbook.model;

public class MemberDto {
	private String userName;
	private String userId;
	private String userPwd;
	private String email;
	private String joinDate;
}
```

- `Alt` + `shift` + `s` : getter and setter 생성

```java
package com.ssafy.guestbook.model;

public class MemberDto {
	
	private String userName;
	private String userId;
	private String userPwd;
	private String email;
	private String joinDate;
	
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	public String getUserId() {
		return userId;
	}
	public void setUserId(String userId) {
		this.userId = userId;
	}
	public String getUserPwd() {
		return userPwd;
	}
	public void setUserPwd(String userPwd) {
		this.userPwd = userPwd;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getJoinDate() {
		return joinDate;
	}
	public void setJoinDate(String joinDate) {
		this.joinDate = joinDate;
	}
	
	@Override
	public String toString() {
		return "MemberDto [userName=" + userName + ", userId=" + userId + ", userPwd=" + userPwd + ", email=" + email
				+ ", joinDate=" + joinDate + "]";
	}
	
}
```

- `public String toString()` : 우리가 직접 객체를 만들었으니까, 그 객체를 프린트 찍을 때 어떤 객체인지 나오고 싶어서 만들었음, 저거 안하면 내용이 안나오고 참조하고 있는 주소를 프린트 찍음

<br>

### 3. Model.mapper 패키지 생성 

#### - MemberMapper 클래스 생성

```java
// MemberMapper.java

@Mapper
public interface MemberMapper {
	
}
```

- `@Mapper` : 이 클래스는 매퍼입니다~, member.xml이랑 연결될꺼야~
- `public interface MemberMapper` : member.xml의 namespace를 여기로 설정하면 member.xml에 있는 sql문을 매핑해서 실행하도록 해주는 친구, xml에 있는 거 가져와서 쓰겠다
- `interface`로 생성하는 클래스는 명세서 느낌으로 생각하자, 프론트에서 개발할 때 아 이런 데이터가 오고 가는구나 이해할 수 있음

<br>

### 4. Model.Service 패키지 생성

#### - MemberService 클래스 생성

```java
public interface MemberService {
	
	MemberDto login(Map<String, String> map) throws Exception;
}
```

- MemberMapper랑 구성이 동일!!
- login 메소드 생성

<br>

#### - MemberServiceImpl 클래스 생성

```java
// MemberServiceImpl.java
@Service
public class MemberServiceImpl implements MemberService {
	
	@Autowired
	private MemberMapper memberMapper;
	
}
```

- `@Service` : 얘 서비스 파일이야 알려주는거
- `@Autowired` : mapper 객체 생성하기 위해서(나중에 메서드 반환 값으로 mapper 객체를 사용하기 때문에)
  - [추후 lombok 방식으로 사용하자](https://velog.io/@deannn/Spring-Boot-maven-%EA%B0%9C%EB%85%90-Lombok-%EC%84%B8%ED%8C%85#getter-setter)

<br>

#### - login 메소드 추가

```java
// MemberServiceImpl.java
@Service
public class MemberServiceImpl implements MemberService {
	
	@Autowired
	private MemberMapper memberMapper;
	
	@Override
	public MemberDto login(Map<String, String> map) throws Exception {
		return memberMapper.login(map);
	}
}
```

<br>

## 5. member.xml 생성 (Mapper 폴더 밑)

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	
<mapper namespace="com.ssafy.guestbook.model.mapper.MemberMapper">

</mapper>
```

- `<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
  	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">` : mybatis 사용하겠노라 선언!
- `<mapper namespace="com.ssafy.guestbook.model.mapper.MemberMapper">` : 여기 있는 명세서를 읽어온다

<br>

#### - login 추가

```xml
<mapper namespace="com.ssafy.guestbook.model.mapper.MemberMapper">
	
	<select id="login" parameterType="map" resultType="MemberDto">
		select username, userid, email
		from ssafy_member
		where userid = #{userId} and userpwd = #{userPwd}
	</select>
	
</mapper>
```

```java
// MemberMapper

@Mapper
public interface MemberMapper {
	MemberDto login(Map<String, String> map) throws Exception;
}
```

- `id` : MemberMapper 에서 생성한 메소드 이름과 동일

- `parameterType` : MemberMapper에서 생성한 id 메소드의 인자로 들어가는 타입 (map가 아니고 Map)
- `resultType` : MemberMapper에서 설정한 반환 값, [참고](https://java119.tistory.com/45)

- `where userid = #{userId} and userpwd = #{userPwd}` : 내가 들고 온 값을 (map 안에 있음)를 `#{}`의 변수로 넣어줘서 체크하기

- `select username, userid, email` : 이 값들을 `MemberDto` 타입으로 넘겨줌

<br>

## 6. MemberController 패키지 생성

### - MemberContorller 클래스 생성

```java
@RestController
@RequestMapping("/user")
public class MemberController {

	@Autowired
	private MemberService memberService;
	
	@PostMapping("/login")
	public ResponseEntity<?> login(@RequestBody Map<String, String> map) throws Exception {
		int auth;
		MemberDto memberDto = memberService.login(map);
		if (memberDto != null) {
			auth = 1;
			return new ResponseEntity<Integer>(auth, HttpStatus.OK) ;
		} else {
			auth = 0;
			return new ResponseEntity<Integer>(auth, HttpStatus.UNAUTHORIZED);
		}
		
	}
}
```

- `@RestController` : 이 클래스 REST컨트롤러야!!
- `@RequestMapping("/user")` : uri가 user인 애 스캔하면 여기야
- `@Autowired` : memberService를 읽어올꺼야
- `@RequestBody`  vs `@RequestParam` : [참고](https://ocblog.tistory.com/49), uri 상에서 데이터가 전달되면 param 그렇지 않으면 body!!
- `ResponseEntity<?>` : 반환 값 json 타입인데 아직 정확히 몰르겠어!
- JWT같은 토큰을 발행해서 할 것 같은데 아직 몰라서 로그인 가능하면 1을 반환 그렇지 않으면 0을 반환!

<BR>

#### 회원가입 성공, JSON 으로 보내야함

![image-20220702153426619](springboot%20%EC%8B%9C%ED%96%89%EC%B0%A9%EC%98%A4.assets/image-20220702153426619.png)



<BR>

#### 로그인 성공, JSON으로 보내줘야 함!

![image-20220702153507589](springboot%20%EC%8B%9C%ED%96%89%EC%B0%A9%EC%98%A4.assets/image-20220702153507589.png)

<br>

## Member 등록, 수정, 삭제, 조회

#### 1. xml 추가

```xml
// member.xml

<select id="login" parameterType="map" resultType="MemberDto">
    select username, userid, email
    from ssafy_member
    where userid = #{userId} and userpwd = #{userPwd}
</select>

<select id="profile" parameterType="String" resultType="MemberDto">
    select * from ssafy_member
    where userid = #{userId}
</select>

<insert id="register" parameterType="memberdto">
    insert into ssafy_member (userid, username, userpwd, email, joindate)
    values (#{userId}, #{userName}, #{userPwd}, #{email}, now())
</insert>

<update id="updateMember" parameterType="MemberDto">
    update ssafy_member set
    userpwd = #{userPwd}, email = #{email} 
    <!-- 		<trim prefix="set" suffixOverrides=",">
   <if test="userPwd != null">userpwd = #{userPwd},</if>
   <if test="email != null">email = #{email},</if>
  </trim> -->
    where userid = #{userId}
</update>

<delete id="deleteMember" parameterType="String">
    delete from ssafy_member
    where userid = #{userId}
</delete>
```

- `trim`을 사용하려는 이유는 조건절을 추가하기 위함
  - if 문으로 컬럼 하나 하나가 비어 있지 않은지 확인하려고!
  - trim이라는 태그 안에 들어 있는 값들이 prefix는 앞에 set 절이라는 명령어 쓰기 위함, suffixOverrides는 가장 마지막 오는 접미사에 해당하는 글자(여기서는 콤마)를 지워버린다는 거임
  - [참고1](https://java119.tistory.com/103), [참고2](https://kimvampa.tistory.com/179)

<br>

#### 2. mapper & memberservice 추가

```java
// MemberService.java & MemberMapper.java

public interface MemberService {
	
	MemberDto login(Map<String, String> map) throws Exception;
	void register(MemberDto memberdto) throws Exception;
	void updateMember(MemberDto memberDto) throws Exception;
	void deleteMember(String userId) throws Exception;
	MemberDto profile(String userId) throws Exception;
}
```

<br>

#### 3. MemberServiceImpl 추가

```java
@Service
public class MemberServiceImpl implements MemberService {
	
	@Autowired
	private MemberMapper memberMapper;
	
	@Override
	public MemberDto login(Map<String, String> map) throws Exception {
		return memberMapper.login(map);
	}
	
	@Override
	public MemberDto profile(String userId) throws Exception {
		return memberMapper.profile(userId);
	}
	
	@Override
	public void register(MemberDto memberdto) throws Exception {
		memberMapper.register(memberdto);
	}
	
	@Override
	public void updateMember(MemberDto memberDto) throws Exception {
		memberMapper.updateMember(memberDto);
	}
	
	@Override
	public void deleteMember(String userId) throws Exception{
		memberMapper.deleteMember(userId);
	}
}
```

<br>

#### 4. MemberController 추가

```java
@RestController
@RequestMapping("/user")
public class MemberController {

	@Autowired
	private MemberService memberService;
	
	
	@PostMapping("/login")
	public ResponseEntity<?> login(@RequestBody Map<String, String> map) throws Exception {
		int auth;
		MemberDto memberDto = memberService.login(map);
		if (memberDto != null) {
			auth = 1;
			return new ResponseEntity<Integer>(auth, HttpStatus.OK) ;
		} else {
			auth = 0;
			return new ResponseEntity<Integer>(auth, HttpStatus.UNAUTHORIZED);
		}
		
	}
	
	
	@GetMapping("/profile")
	public ResponseEntity<?> profile(@RequestBody String userId) throws Exception {
		MemberDto memberDto = memberService.profile(userId);
		return new ResponseEntity<MemberDto>(memberDto, HttpStatus.OK);
		
	}
	
	@PostMapping("/register")
	public ResponseEntity<?> register(@RequestBody MemberDto memberDto) throws Exception{
		memberService.register(memberDto);
		return new ResponseEntity<Void>(HttpStatus.OK);
	}

	@PutMapping("/register")
	public ResponseEntity<?> updateMember(@RequestBody MemberDto memberDto) throws Exception{
		memberService.updateMember(memberDto);
		return new ResponseEntity<Void>(HttpStatus.OK);
	}
	
	
	@DeleteMapping("/register")
	public ResponseEntity<?> deleteMember(@RequestBody String userId) throws Exception{
		memberService.deleteMember(userId);
		return new ResponseEntity<Void>(HttpStatus.OK);
	}
	
}
```

<br>

## 추가세팅

### 1. context-path

```properties
# application.properties 파일
#server setting
server.servlet.context-path=/api/v1
```

- `server.servlet.context-path` : 여기에 설정한 값들이 `uri` 의 prefix라고 생각하면 돼



<br>



# JPA

### 환경설정

#### - properties

```properties
# application.properties

#server setting
server.port=80
server.servlet.context-path=/api/v1

#DataBase Setting
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url= jdbc:mysql://127.0.0.1:3306/ssafyweb?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8
spring.datasource.username=ssafy
spring.datasource.password=ssafy
```

<br>

#### - pom.xml

```xml
<dependencies>
    ...
    <!-- lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```



<br>

### 1. entity 생성

```java
// entity.java

@Builder
@Data
@Entity(name="ssafy_member")
//@Table(name="ssafy_member")
public class entity {
	
	private String userId;
	private String userName;
	private String userPwd;
	private String email;
	private String joinDate;
}
```

- `@Builder` : SQL 사용 시 파라미터에 값을 쉽게 넣어주기 위한 어노테이션
- `@Data` : getter, setter, toStirng, 생성자를 자동으로 완성시켜주는 어노테이션
  - @Getter
   * @Setter
   * @RequiredArgsConstructor
   * @ToString
   * @EqualsAndHashCode
   * @lombok.Value
- `@Entity`
  - [참고](https://gmlwjd9405.github.io/2019/08/11/entity-mapping.html)

<br>

