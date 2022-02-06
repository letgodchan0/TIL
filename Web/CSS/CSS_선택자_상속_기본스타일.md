# 🌱 CSS - 선택자, 상속, 기본 스타일

### CSS

- Cascading Style Sheets

- 스타일을 지정하기  위한 언어
- 선택하고, 스타일을 지정하는 것으로 기본 구문은 아래와 같다.

![image-20220205212451439](CSS_%EC%84%A0%ED%83%9D%EC%9E%90_%EC%83%81%EC%86%8D_%EA%B8%B0%EB%B3%B8%EC%8A%A4%ED%83%80%EC%9D%BC.assets/image-20220205212451439.png)



- 기본적으로 선택자를 통해 스타일을 지정할 HTML 요소를 선택한다.
- 중괄호 안에서 속성과 값 하나의쌍으로 이루어진 선언을 진행한다.
- 각 쌍은 선택한 요소의 속성, 속성에 부여할 값을 의미한다.
  - 속성 (Property) : 어떤 스타일 기능을 변경할지 결정
  - 값 (Value) : 어떻게 스타일 기능을 변경할지 결정

### CSS 정의 방법

1. 인라인 방식으로 해당 태그에 직접 style 속성을 활용한다.

![image-20220205212823575](CSS_%EC%84%A0%ED%83%9D%EC%9E%90_%EC%83%81%EC%86%8D_%EA%B8%B0%EB%B3%B8%EC%8A%A4%ED%83%80%EC%9D%BC.assets/image-20220205212823575.png)



2. 내부 참조 방식으로 `<head>` 태그 내에 `<style>` 에 지정하는 방법이다.

![image-20220206154755891](CSS_%EC%84%A0%ED%83%9D%EC%9E%90_%EC%83%81%EC%86%8D_%EA%B8%B0%EB%B3%B8%EC%8A%A4%ED%83%80%EC%9D%BC.assets/image-20220206154755891.png)



3. 외부 참조 방식으로 외부의 CSS 파일을 `<head>` 내 `<link>` 를 통해 불러오기 때문에 수정 보완이 편하고 가장 많이 활용한다.

![image-20220205213050123](CSS_%EC%84%A0%ED%83%9D%EC%9E%90_%EC%83%81%EC%86%8D_%EA%B8%B0%EB%B3%B8%EC%8A%A4%ED%83%80%EC%9D%BC.assets/image-20220205213050123.png)



## 선택자 (Selectors)

![image-20220205212451439](CSS_%EC%84%A0%ED%83%9D%EC%9E%90_%EC%83%81%EC%86%8D_%EA%B8%B0%EB%B3%B8%EC%8A%A4%ED%83%80%EC%9D%BC.assets/image-20220205212451439.png)





### 선택자 유형

- 기본 선택자
  - 전체 선택자, 요소 선택자
  - 클래스 선택자, 아이디 선택자, 속성 선택자
- 결합자(Combinators)
  - 자손 결합자, 자식 결합자
  - 일반 형체 결합자, 인접 형제 결합자
- 의사 클래스/요소(Pseudo Class)
  - 링크, 동적 의사 클래스
  - 구조적 의사 클래스, 기타 의사 클래스, 의사 엘리먼트, 속성 선택자

![image-20220205214219453](CSS_%EC%84%A0%ED%83%9D%EC%9E%90_%EC%83%81%EC%86%8D_%EA%B8%B0%EB%B3%B8%EC%8A%A4%ED%83%80%EC%9D%BC.assets/image-20220205214219453.png)





### 선택자 정리

- `*` : 전체 선택자
- `태그명(h1 or p)` : 요소 선택자, HTML 태그를 직접 선택
- `.class` : class 선택자
  - 일반적인 스타일링 시 사용되는 것
  - 마침표(.)문자로 시작하며, 해당 클래스가 적용된 항목을 선택
- `#id` : id 선택자
  - `#` 문자로 시작하며 해당 아이디가 적용된 항목을 선택
  - 일반적으로 하나의 문서에 1번만 사용, 여러번 사용해도 동작하지만 단일 id를 사용하는 것을 권장

- `div > .childern` : 자식 선택자
  - div 태그 바로 밑에 있는 children 클래스를 선택
- `div .baby` : 자손 선택자
  - div 태그 하위 모든 baby 클래스를 선택

### 선택자 적용 우선순위

1. `!important` 
   - 최강 우선순위, 사용시 주의!

2. 인라인 `style` 
   - 해당 라인에 직접적으로 적용하는 것
3. `#id` 
4. `.class`
5. 태그명 (h1 or p)
6. `*` 전체 선택

> 같은 레벨인 경우 CSS 파일에서 나중에 선언된 것이 적용된다.

## 선택자 심화

### 결합자 (Combinators)

- 자손 결합자

  - selectorA **하위의 모든** selectorB 요소

  ```css
  # css
  # div 하위의 모든 span 요소 선택 적용
  div span {
      color: red;
  }
  ```

  ```html
  # html
  <div>
      <span>이건 빨강입니다.</span>
      <p>이건 빨강이 아닙니다.</p>
      <p>
          <span>이건 빨강입니다.</span>
      </p>
  </div>
  ```

  

- 자식 결합자

  - selectorA **바로 아래의** selectorB 요소

  ```css
  # css
  # div 바로 아래의 span 요소 선택 적용
  div > span {
      color: red;
  }
  ```

  ```html
  # html
  <div>
      <span>이건 빨강입니다.</span>
      <p>이건 빨강이 아닙니다.</p>
      <p>
          <span>이건 빨강이 아닙니다.</span>
      </p>
  </div>
  ```

  

- 일반 형제 결합자

  - selectorA의 형제 요소 중 뒤에 위치하는 selectorB 요소를 모두 선택

  ```css
  # css
  # p의 형제 요소 중 뒤에 위치하는 span 요소를 모두 선택
  p ~ span {
      color: red;
  }
  ```

  ```html
  # html
  <span>p 태그 앞에 있기 때문에 이건 빨강이 아닙니다.</span>
  <p>여기 문단이 있습니다.</p>
  <b>그리고 코드도 있습니다. p와 형제 요소이긴 하지만 선택 받지 못함</b>
  <span>p태그와 형제이기 때문에 이건 빨강입니다!</span>
  <b>더 많은 코드가 있습니다. p와 형제 요소이긴 하지만 선택 받지 못함</b>
  <span>이것도 p태그와 형제이기 때문에 이건 빨강입니다!</span>
  ```

- 인접 형제 결합자

  - selectorA의 형제 요소 중 바로 뒤에 위치하는 selectorB 요소를 선택

  ```css
  # css
  # p의 형제 요소 중 바로 뒤에 위치하는 span 요소 하나를 선택
  p + span {
      color: red;
  }
  ```

  ```html
  # html
  <span>p 태그 앞에 있기 때문에 이건 빨강이 아닙니다.</span>
  <p>여기 문단이 있습니다.</p>
  <b>그리고 코드도 있습니다. p와 형제 요소이긴 하지만 선택 받지 못함</b>
  <span>p태그와 인접한 형제이기 때문에 이건 빨강입니다!</span>
  <b>더 많은 코드가 있습니다. p와 형제 요소이긴 하지만 선택 받지 못함</b>
  <span>p태그와 인접한 형제가 아니기 때문에 이건 빨강이 아닙니다!</span>
  ```

  

## CSS 상속

- CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속한다.
  - 속성(프로퍼티) 중에는 상속이 되는 것과 되지 않는 것들이 있다.
  - 상속되는 속성
    - Text 관련 요소 (font, color, text-align), opacity, visibility 등 style과 관련된 것
  - 상속되지 않는 속성
    - Box model (width, height, margin, padding, border, box-sizing, display)
    - Position 관련 요소 (position, top/right/bottom/left, z-index) 등

```html
# html 파일
<body>
    <p>안녕하세요 <span> 반가워요</span> 잘있어요</p>
</body>
```

```css
# css 파일
<style>
p {
    /* 상속됨 */
    color : red;
    /* 상속 안됨 */
    border: 1px solid black;
}
```

## CSS 기본 스타일

### 크기 단위

- px (픽셀)
  - 모니터 해상도의 한 화소인 '픽셀 기준'
  - 픽셀의 크기는 변하지 않기 때문에 고정적인 단위
- %
  - 백분율 단위
  - 가변적인 레이아웃에서 자주 사용
- em
  - (바로 위, 부모 요소에 대한) 상속의 영향을 받음
  - 배수 단위, 요소에 지정된 사이즈에 상대적인 사이즈를 가짐
  - 부모 요소의 폰트가 24px이고`font-size: 1.5em;` 으로 준다면 폰트 사이즈는 24px * 1.5는 36px이 된다.
- rem
  - (바로위, 부모 요소에 대한) 상속의 영향을 받지 않음
  - 최상위 요소(html)의 사이즈를 기준으로 배수 단위를 가짐
  - 부모 요소의 폰트가 24px이어도 최상위 요소인 root의 폰트 사이즈인 16px * 1.5 를 적용해서 24px가 된다.
- viewport
  - 웹 페이지를 방문한 유저에게 바로 보이게 되는 웹 컨텐츠의 영역 (디바이스 화면)
  - 디바이스의 viewport를 기준으로 상대적인 사이즈가 결정됨
  - vw, vh, vmin, vmax

### 색상 단위

- 색상 키워드

  - 대소문자를 구분하지 않음
  - red, blue, black과 같은 특정 색을 직접 글자로 나타낸다.

  ```css
  p { color : black;}
  ```

  

- RGB 색상

  - 16진수 표기법 혹은 함수형 표기법을 사용해서 특정 색을 표현하는 방식

  ```css
  p { color : #000;}  '#' + 16진수 표기법
  p { color : #000000;}  '#' + 16진수 표기법
  
  p { color : rgb(0, 0, 0);}	rgb() 함수형 표기법
  
  p { color : rgba(0, 0, 0, 0.5);}	rgba() 함수형 표기법, a는 alpha(투명도)
  ```

- HSL 색상

  - 색상, 채도, 명도를 통해 특정 색을 표현하는 방식

  ```css
  p { color : hsl(120, 100%, 0.5);}  HSL (색상, 채도, 명도)
  
  p { color : hsla(120, 100%, 0.5);}  HSL (색상, 채도, 명도, 투명도) a는 alpha(투명도)
  ```

### 문서 표현 (이런거 있다는 것만 알고 있자)

- 텍스트
  - 서체, 서체 스타일
  - 자간, 단어 간격, 행간, 들여쓰기 등
- 컬러(color), 배경(background-image, background-color)
- 기타 HTML 태그별 스타일링
  - 목록(li), 표(table)