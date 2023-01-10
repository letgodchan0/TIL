# Spring boot 환경설정 및 CRUD

<br>

## - 환경설정 시행착오

### 1. Lombok 적용

![image-20220713001107281](README.assets/image-20220713001107281.png)

- gradle 프로젝트로 import를 한 후 서버를 실행했을 때 lombok 관련 에러가 발생했다. 명세서대로 lombok를 설치했지만, 적용되지 않았고 다음과 같이 Project and External Dependencies에서 직접 추가해주었다.

<br>

### 2. 경로 변경

![image-20220713012147551](README.assets/image-20220713012147551.png)

- `lombok` 추가 후 확인 차 서버를 실행해 봤는데, 서버 에러가 발생했다. 문제는 front 소스 중 `vue.config.js`의 `outputDir`의 `backend-java`로 변하면 해결된다. 

<br>

### 3. Access denied for user

![image-20220713002134846](README.assets/image-20220713002134846.png)

- 한번 더 서버를 실행 했을 때는, `Access denied for user ''@localhos` 에러가 발생했다. DB 접근권한을 부여하는 아이디와 패스워드를 생성한뒤 직접 입력해주어야 했다. 나는 `admin`으로 설정했다!

<br>

### 4. PostCss

![image-20220713003521418](README.assets/image-20220713003521418.png)

```bash
npm rebuild node-sass
```

- vue에서 scss와 같은 css 형식을 사용하기 위해 sass-loader를 설치한 뒤에 실행하면 위와 같은 오류가 나는 경우가 있다. sass 관련 환경을 구성하기 위해 node-sass 모듈을 설치하거나 위와 같이 `rebuild` 하면 해결된다. 

<br>

### 성공!

![image-20220713002256793](README.assets/image-20220713002256793.png)

<br>

## - Back-end 요구사항

### 1. 회원가입

```java
// UserServiceImpl.java

@Override
public User createUser(UserRegisterPostReq userRegisterInfo) {
    User user = new User();
    user.setUserId(userRegisterInfo.getId());
    user.setPassword(passwordEncoder.encode(userRegisterInfo.getPassword()));
    // 아래 추가
    user.setDepartment(userRegisterInfo.getDepartment());
    user.setName(userRegisterInfo.getName());
    user.setPosition(userRegisterInfo.getPosition());
    user.setUserId(userRegisterInfo.getUserId());
    return userRepository.save(user);
}
```

- 회원 가입을 위해 컨트롤러를 쭉 따라가다 보면 현재 `id`와 `password`만 가져오고 있다. user 테이블의 컬럼들과 매핑되는 값들을 가져오도록 한다.

<br>

```java
// UserRegisterPostReq

public class UserRegisterPostReq {
	@ApiModelProperty(name="유저 ID", example="ssafy_web")
	String id;
	@ApiModelProperty(name="유저 Password", example="your_password")
	String password;
    // 아래 추가
	String department;
	String name;
	String position;
    String userId;
}
```

- `createUser`에서 `registerInfo`를 가져오려면,  user 테이블과 매핑되는 컬럼을 작성해주어야 한다. 처음에는 `id`와 `password`만 선언되어 있었기 때문에 user table에 필요한 컬럼들에 해당하는 값들을 추가적으로 선언해 준다.

<br>

![image-20220713142215543](README.assets/image-20220713142215543.png)

- db에 잘 들어오는 것을 확인할 수 있다!

<br>

### 2. 로그인

```java
// AuthController.java

public ResponseEntity<UserLoginPostRes> login(@RequestBody @ApiParam(value="로그인 정보", required = true) UserLoginPostReq loginInfo) {
		String userId = loginInfo.getUserId();
    
// UserLoginPostRequest
    @ApiModelProperty(name="유저 ID", example="ssafy_web")
	String userId;
```

- 로그인 요청을 보냈을 때 서버 에러가 발생했다. `getUserByUserId`는 `userId`가 필요한데 `loginInfo`에서 그냥 `id`를 가져오고 있었다. 따라서 위와 같이 id가 아닌 userId를 가져오도록 수정해야 한다

<br>

### + 추가

![image-20220713151254861](README.assets/image-20220713151254861.png)

- 추가적으로 Jwt 토큰을 헤더에 넣는 경우 Token이 아닌 `Bearer`와 토큰을 넣어야 한다. 이 부분은 장고랑 달라서 조금 헤맸다!

<br>

### 3. 프로필

```json
{
    "user_id" : 1
}
```

```java
// UserRes.java

@Getter
@Setter
@ApiModel("UserResponse")
public class UserRes{
	@ApiModelProperty(name="User ID")
	String userId;
	String name;
	String position;
	String deparement;
	
	public static UserRes of(User user) {
		UserRes res = new UserRes();
		res.setUserId(user.getUserId());
		res.setName(user.getName());
		res.setPosition(user.getPosition());
		res.setDeparement(user.getDepartment());
		return res;
	}
}
```

- `api/v1/users/me`로 get 요청을 보냈을 때 id만 반환해주고 있었다. 요구사항에서는 id 뿐만 아니라 deparment, position, name도 같이 반한되어야 했다. 컨트롤러에서 반환해주는 타입이 `UseRes` 타입이었고 여기에서 나머지 3개를 추가적으로 정의한 후 set해온 뒤 반환을 해주면 요구사항대로 다 받아올 수 있다.

<br>

### 4. 존재하는 회원 확인

```java
// UserRepository.java

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
	...,
    // 추가
    boolean existsByUserId(String userId);
}
```

- JPA는 기본적으로 여러 메서드를 제공하고 있는 것 같다. 그 중 existsBy(찾으려는 값)은 알아서 해당 값이 존재하는 지 조회하는 select 쿼리를 날려준다..ㄷㄷ

<br>

```java
// UserController.java

@GetMapping("/{userId}")
	public ResponseEntity<?> checkUserId(@PathVariable String userId) throws Exception{

		if (userService.existsByUserId(userId) == true) {
			return ResponseEntity.status(409).body(BaseResponseBody.of(409, "이미 존재하는 사용자 ID 입니다."));
		}else {
			return ResponseEntity.status(200).body(BaseResponseBody.of(200, "성공!"));
		}
	}
```

- `uri`에 담아서 오는 userId를 인자로 받기 위해서는 `@PathVariable` 어노테이션이 필요하다.
- 위에 정의한 `existsByUserId`를 통해 분기처리를 한 후 알맞게 반응해주면 된다.

<br>

![image-20220714032015391](README.assets/image-20220714032015391.png)

<br>



### 5. 회원정보 수정

```java
// UserController.java

@PatchMapping("/{userId}")
	public ResponseEntity<?> update(@PathVariable String userId, @RequestBody UserUpdatePostReq updateInfo){
		
		if (userService.existsByUserId(userId) == true) {
			// 유저 정보 수정
			userService.updateUserInfo(userId, updateInfo);
			return ResponseEntity.status(200).body(BaseResponseBody.of(200, "성공!"));
		}else {	
			return ResponseEntity.status(400).body(BaseResponseBody.of(400, "존재하지 않는 회원입니다."));
		}
	}
```

- 경로에서 userId를 가져오고 넘겨주는 데이터를 미리 정의 해둔 `UserUpdatePostReq` 타입으로 받아온다. 여기에는 명세서에서 요구한대로 description, position, name가 정의되어 있다. 
- 가져온 userId로 유저 아이디 체크 메서드를 통해 분기를 한 후 존재하는 유저 아이디라면 `updateUserInfo` 메서드를 실행시키게 된다.

<br>

```java
// UserServiceImpl.java

public User updateUserInfo(String userId, UserUpdatePostReq updateInfo) {
		Optional<User> user = userRepository.findByUserId(userId);
		if (!user.isPresent()) {
			throw new EntityNotFoundException("존재하지 않는 회원인데?");
		}
		User newUser = user.get();
		newUser.setDepartment(updateInfo.getDepartment());
		newUser.setPosition(updateInfo.getPosition());
		newUser.setName(updateInfo.getName());
		return userRepository.save(newUser);
	}
```

- `Optional<User> user = userRepository.findByUserId(userId);` : 먼저 useId 에 해당하는 user 객체를 가져와서 존재하지 않는 회원이라면 예외처리를 해준다.
- `User newUser = user.get();` : newUser에 인자로 가져온 userId에 해당하는 데이터들을 담아주고 set을 통해 값을 바꿔준다!

<br>

### 6. 회원 삭제

```java
// UserRepository.java

public interface UserRepository extends JpaRepository<User, Long> {
	...
    @Transactional
    void deleteByUserId(String userId);
}
```

- 기본적으로 JPA는 delete와 deleteId를 제공하고 있다. [참고](https://www.baeldung.com/spring-data-jpa-delete) 
- 내가 원하는데로 custom을 해주고 싶다면 `deleteBy` + `내가 원하는 값`의 형태로 메서드를 정의하면 되고, 여기서는 userId에 해당하는 데이터를 제거하려고 한다.
- `@Transactional` : DB에 접근할 때 읽기는 transaction에 크게 영향을 받지 않지만, delete와 insert 같은 DB에 직접적인 변화를 주는 행동은 transaction에 민감하기 때문에 어노테이션으로 꼭 달아주어야 한다!! 

<br>



### 7. Entity 생성

![](README.assets/image-20220715011756805.png)

- JPA에서 엔티티란 DB 테이블에 대응하는 하나의 클래스라고 생각할 수 있다. `@Entity`가 붙은 클래스는 JPA가 관리해주며, JPA를 사용해서 DB 테이블과 매핑할 클래스는 `@Entity`를 붙여야만 매핑이된다. 그리고 STS에서 엔티티를 생성해주면 `mysql` 테이블에 자동으로 생성되는 것을 확인할 수 있다.
- Entity 주의사항
  - 기본 생성자(파라미터가 없는 생성자)필수, 접근제어자는 public 혹은 protected이어야 함.
  - final 클래스, enum, interface, inner 클래스에는 사용이 불가
  - 저장할 필드에는 final을 붙이면 안된다!! 테이블과 매핑되지 않음!
