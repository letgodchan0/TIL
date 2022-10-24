# 🌱 Web

## HTML

- Hyper Text: 참조(하이퍼링크)를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
- Markup Language: 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어 (HTML, Markdown)
- **웹 페이지를 작성(구조화)하기 위한 하나의 언어**

## HTML 기본 구조

- html : 문서의 최상위(root) 요소
- head : 문서 메타데이터 요소
  - 문서 제목, 인코딩, 스타일, 외부 파일 로딩 등
  - 일반 적으로 브라우저에 나타지 않는 내용
  - `<title>` : 브라우저 상단  타이틀
  - `<meta>` : 문서 레벨 메타데이터 요소
  - `<link>` : 외부 리소스 연결 요소 (CSS 파일, favicon 등)
  - `<script>` : 스크립트 요소 (JavaScript 파일/코드)
  - `<style>` : CSS 직접 작성

```html
<head>
    <title>HTML 예시</title>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet">
    <script src="javascript.js"></script>
    <style>
    p {
        color : black;
    }
    </style>
</head>
```



- body : 문서 본문 요소
  - 실제 화면 구성과 관련된 내용



### DOM 트리

- 텍스트 파일인 HTML 문서를 브라우저에서 렌더링 하기 위한  구조 (브라우저에 만들기 위한)
- body 태그를 부모 태그, 그 밑에 있는 애들은 자식 태그 (자식 태그끼리는 sibling 태그)

```html
<body>
    <h1> 웹문서 </h1>
    <ul>
        <li>HTML</li>
        <li>CSS</li>
    </ul>
</body>
```



### 요소(element)

- HTML 요소는 시작 태그와 종료 태그 그리고 태그  사이에 위치한 내용으로 구성	
  - `<html> contents </h1>`
  - 태그(Element,  요소)는 컨텐츠(내용)를 감싸는 것으로 그 정보의 성격과 의미를 정의

- 내용이 없는 태그들은 닫는 태그가 없음!
  - br: 엔터
  - hr: 수평선
  - img: 이미지 링크 달아두기
  - input: 사용자한테 입력받기
  - link: 주소정보를 html 태그 상에 담으려고
  - meta: 문서 레벨 메타데이터 요소, 관련된 내용을 담음
  
- 요소의 중첩을 통해 하나의 문서를 구조화 (DOM 트리)

### 속성(attribute)

`<a href="https://google.com"></a>`

- href : 속성명
- https://google.com : 속성값

- 속성명 뒤에 공백 없이 쌍따옴표를 사용하자!
- 속성을 통해 태그의 부가적인 정보를 설정할 수 있음
- 요소의 시작 태그에 작성하며 보통 이름과 값이 하나의 쌍으로  존재
- 태그와 상관없이 사용 가능한 속성(HTML Global Attribute)들도 있음

#### HTML Global Attribute

> 모든 HTML 요소가 공통으로 사용할 수 있는 대표적인 속성 (몇몇 요소에는 아무 효과 없을 수 있다!)

- **id** : 문서 전체에서 유일한 고유 식별자 지정
- **class** : 공백으로 구분된 해당 요소의 클래스의 목록 (CSS, JS에서 요소를 선택하거나 접근)
- data-* : 페이지에 개인 사용자 정의 데이터를 저장하기 위해 사용
- **style** : inline 스타일
- title : 요소에 대한 추가 정보 지정
- tabindex : 요소의 탭 순서

### 시맨틱 태그

> 좀 편하게 하려고 영역(div)마다 다른 이름을 붙여주었음

- HTML5에서 의미론적 요소를 담은 태그의 등장
  - 기존 영역을 의미하는 div 태그를 대체하며 사용
- 대표적인 태그 목록
  - header - 문서 전체나 섹션의 헤더(머리말 부분)
  - nav : 내비게이션
  - aside : 사이드에 위치한 공간, 메인 컨텐츠와 관련성이 적은 컨텐츠
  - section : 문서의 일반적인 구분, 컨텐츠의 그룹을 표현
  - article : 문서, 페이지, 사이트 안에서 독립적으로 구분되는 영역
  - footer : 문서 전체나 섹션의 푸터(마지막 부분)


- 구체적
  - Non semantic 요소로는 div, span 등이 있으며 h1, table 태그들도 시맨틱 태그로 볼 수 있음
  - 개발자 및 사용자 뿐만 아니라 검색엔진 등에 의미 있는 정보의 그룹을 태그로 표현
  - 단순히 구역을 나누는 것 뿐만 아니라 '의미'를 가지는 태그들을 활용하기 위한 노력
  - 요소의 의미가 명확해지기 때문에 코드의 가독성을 높이고 유지보수를 쉽게 함 
  - 검색엔진 최적화를 위해서 메타태그, 시맨틱 태그 등을 통한 마크업을 효과적으로 활용 해야함

### HTML 문서 구조화

#### 텍스트 요소

- `<a></a>` : href 속성을 활용하여 다른 URL로 연결하는 하이퍼링크 생성
- `<b></b>` : 굵은 글씨 요소, 중요하고 강조하고자 하는 요소
- `<strong></strong>`: 굵은 글씨 요소, 중요하고 강조하고자 하는 요소 (시맨틱 태그)
- `<i></i>`: 기울임 글씨 요소, 중요하고 강조하고자 하는 요소
- `<em></em>`: 기울임 글씨 요소, 중요하고 강조하고자 하는 요소 (시맨틱 태그)
- `<br>`: 텍스트 내에 줄 바꿈 생성
- `<img>`: src 속성을 활용하여 이미지 표현
- `<span></span>`: 의미 없는 인라인 컨테이너

#### 그룹 컨텐츠

- `<p></p>`: 하나의 문단
- `<hr>`: 문단 레벨 요소에서의 주제의 분리를 의미하며 수평선으로 표현됨
- `<ol></ol>`: 순서가 있는 리스트
- `<ul></ul>`: 순서가 없는 리스트
- `<pre></pre>`: HTML에 작성한 내용을 그대로 표현, ```이거 썼을 때
- `<blockquote></blockquote>`: 텍스트가 긴 인용문 주로 들여쓰기를 한 것으로 표현됨 > 이거 썼을 때
- `<div></div>`: 의미 없는 블록 레벨 컨테이너

#### table

- table의 각 영역을 명시하기 위해 `<thead>`, `<tbody>`, `<tfoot>` 요소를 활용

![image-20220203210445129](HTML_%EA%B8%B0%EB%B3%B8%EA%B5%AC%EC%A1%B0_%EB%AC%B8%EB%B2%95.assets/image-20220203210445129.png)

- `<tr>` 으로 가로 줄을 구성하고 내부에는 `<th>` 혹은 `<td>`로 세을 구성한다.
- colspan, rowspan 속성을 활용하여 셀을 병합한다.
- `<caption>`을 통해 표 설명 또는 제목을 나타낸다.

#### form

- 서버에 정보(데이터)를 전송해주는 역할을 한다.
- 기본 속성
  - action : form을 처리할 서버의 URL
  - method : form을 제출할 때 사용할 HTTP 메서드 (GET 혹은 POST)
  - enctype: method가 post인 경우 데이터의 유형 (우리가 홈페이지에 파일 같은거 첨부할 때 사용)


```html
<form action="/search" method='GET'>
    # search가 상대경로 느낌으로 앞에 url이 있음
</form>
```

#### input

- 다양한 타입을 가지는 입력 데이터 유형과 위젯이 제공됨
- 기본 속성
  - name : form control에 적용되는 이름
  - value : form control에 적용되는 값
  - 보통 name과 value가 페어로 전송된다
  - required, readonly, autofocus, autocomplete, disabled 등

```html
<form action="/search" method='GET'>
    <input type="text" name="q">
</form>   
```

#### input label

- label을 클릭하여 input 자체의 초점을 맞추거나 활성화 시킬 수 있음
  - 사용자는 선택할 수 있는 영역이 늘어나 웹 / 모바일(터치) 환경에서 편하게 사용할 수 있음
  - label과 input 입력의 관계가 시각적 뿐만 아니라 화면 리더기에서도 label을 읽어 쉽게 내용을 확인할 수 있도록 한다
- `<input>`에 id 속성을, `<label>`에는 for 속성을 활용하여 상호 연관을 시킨다.

```html
<label for="agreement">개인정보 수집 동의</label>
<input type="check" name="agreement" id="agreement">
id를 각각 동일한 식별자로 매핑하면 상호 연관시킬 수 있다
```

#### input 유형

1. 일반

- 일반적으로 입력을 받기 위하여 제공되며 타입별로 HTML 기본 검증 혹은 추가 속성을 활용할 수 있음
- text : 일반 텍스트 입력
- password : 입력 시 값이 보이지 않고 문자를 특수기호(*)로 표현
- email : 이메일 형식이 아닌 경우 form 제출 불가
- number : min, max, step 속성을 활용하여 숫자 범위 설정 가능
- file : accept 속성을 활용하여 파일 타입 지정 가능

2. 항목 중 선택

- 일반적으로 label을 사용하여 내용을 작성하여 항목 중 선택할 수 있는 input을 제공
- 동일  항목에 대하여는 동일한 name을 지정하고 선택된 항목에 대한 value를 지정해야 함
  - checkbox : 다중 선택
  - radio : 단일 선택

3. 기타

- 다양한 종류의 input을 위한 picker를 제공
  - color : color picker
  - date : date picker
- hidden input을 활용하여 사용자 입력을 받지 않고 서버에 전송되어야 하는 값을 설정
  - hidden : 사용자에게 보이지 않는 input
