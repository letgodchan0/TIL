# 📒 JavaScript - DOM 조작

<br>







### 📚 DOM 선택 관련 메서드 (1)

```js
const nav = document.querySelector('nav')	// selector 가져오기
const button = document.querySelector('#submitButton')	// id는 유일하기 때문에 id 가져오기
const navImg = document.querySelector('nav > img')	// jspath 입력

const questionDivs = document.querySelectorAll('section div') // section의 자손 중 div 가져오기
```

- `document.querySelector(selector)`
  - 제공한 선택자와 일치하는 element 하나 선택
  - 제공한 CSS selector를 만족하는 첫 번째 element `객체`를 반환 (없다면 null)
- `document.querySelectorAll(selector)`
  - 제공한 선택자와 일치하는 여러 element를 선택
  - 매칭 할 하나 이상의 셀렉터를 포함하는 유효한 CSS selector를 인자(문자열)로 받음
  - 지정된 셀렉터에 일치하는 `NodeList`를 반환

<br>

### 📚 DOM 선택 관련 메서드 (2)

- `getElementById(id)` - 단일 element 객체 반환
- `getElementsByTagName(name)`  - HTML Collection 반환
- `getElementsByClassName(names)` -  HTML Collection 반환

>  선택 관련 메서드 중 이런 애들도 있지만 querySelector(), querySelectorAll()만 사용해야 한다!!
>
> - id, class 그리고 tag 선택자 등을 모두 사용 가능하므로, 더 구체적이고 유연하게 선택 가능

<br>

### 💡 HTMLCollection & NodeList

> 둘 다 배열과 같이 각 항목에 접근하기 위한 index를 제공 (유사 배열)

- HTMLCollection
  - name, id, index 속성으로 각 항목에 접근 가능
- NodeList
  - index로만 각 항목에 접근 가능
  - 단, HTMLCollection과 달리 배열에서 사용하는 `forEach` 메서드 및 다양한 메서드 사용 가능

- "둘 다 Live Collection으로 DOM의 변경사항을 실시간으로 반영하지만, querySelectorAll에 의해서만 반환되는 `NodeList`는 Static Collection으로 실시간으로 반영되지 않음!!!!"

<br>

### 📚 Collection

- Live Collection
  - 문서가 바뀔 때 실시간으로 업데이트 됨
  - DOM의 변경사항을 실시간으로 collection에 반영
    - HTMLCollection, NodeList
- Static Collection (non-live)
  - DOM이 변경되어도 collection 내용에는 영향을 주지 않음
  - querySelectorALL()의 반환 NodeList 만 static collection!!!!!!!!!!!!!!!!!!!!!!!!

<br>



### 🌈 DOM 변경 관련 메서드 (Creation)

```js
document.createElement()
const footer = document.createElement('footer')
```

- 작성한 태그 명의 HTML 요소를 생성하여 반환!

<br>

### 🌈 DOM 변경 관련 메서드 (append DOM)

#### Element.append()

```js
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')
newLiTag.innerText = '새로운 리스트 태그'
ulTag.append(newLiTag)
ulTag.append('그냥 이런 문자열도 추가 가능!')

const new1 = document.createElement('li')
new1.innerText = '리스트 1'
const new2 = document.createElement('li')
new2.innerText = '리스트 2'
const new3 = document.createElement('li')
new3.innerText = '리스트 3'
ulTag.append(new1, new2, new3)
```

- 특정 부모 Node의 자식 NodeList 중 마지막 자식 다음에 Node 객체나 DOMString을 삽입
- 여러 개의 Node 객체, DOMString을 추가할 수 있음
- 반환 값이 없음



<hr>

#### Node.appendChild()

```js
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')
newLiTag.innerText = '새로운 리스트 태그'
ulTag.appendChild(newLiTag)
// ulTag.appendChild(newLiTag, newLiTag2) 여러개 불가, but js 특징 함수 안에 인자 많아도 에러 x
ulTag.appendChild('문자열은 추가 불가')

const new1 = document.createElement('li')
new1.innerText = '리스트 1'
const new2 = document.createElement('li')
new2.innerText = '리스트 2'
ulTag.appendChild(new1, new2)	// new1만 추가된다! 
```

- 한 Node를 특정 부모 Node 자식 NodeList 중 마지막 사직으로 삽입 (Node만 추가 가능)
- 한번에 오직 하나의 Node만 추가할 수 있음
- 만약 주어진 Node가 이미 문서에 존재하는 다른 Node를 참조한다면 새로운 위치로 이동

<hr>

#### 💡ParentNode.append() vs Node.appendChild()

- `append()`를 사용하면 DOMString 객체를 추가할 수도 있지만, `.appendChild()`는 Node 객체만 허용
- `append()`는 반환 값이 없지만, `appendChild()`는 추가된 Node 객체를 반환
- `append()`는 여러 Node 객체와 문자열을 추가할 수 있지만 `appendChild()`는 하나의 Node 객체만 추가할 수 있음 

<br>

### 🌈 DOM 변경 관련 속성 (property)

```js
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')

liTag1.innerText = '<strong>피자</strong>'	// <strong>피자</strong> 그대로 입력
liTag2.innerHTML = '<strong>피자</strong>'	// 피자가 볼드체 되어서 입력된다!
```

- `Node.innerText`
  - Node 객체와 그 자손의 텍스트 컨텐츠(DOMString)를 표현 (해당 요소 내부의 raw text) (사람이 읽을 수 있는 요소만 남김)
  - 즉, 줄 바꿈을 인식하고 숨겨진 내용을 무시하는 등 최종적으로 스타일링이 적용된 모습으로 표현
- `Element.innerHTML`
  - 요소(element) 내에 포함된 HTML 마크업을 반환
  - [참고] XSS 공격에 취약하므로 사용시 주의!!!!

<br>

### 💡 XSS (Cross-site Scripting)

- 공격자가 입력요소를 사용하여 (`<input>`) 웹 사이트 클라이언트 측 코드에 악성 스크립트를 삽입해 공격하는 방법
- 피해자(사용자)의 브라우저가 악성 스크립트를 실행하며 공격자가 엑세스 제어를 우회하고 사용자를 가장 할 수 있도록 함

<br>



### 🌈 DOM 삭제 관련 메서드

```js
const header = document.querySelector('#location-header')
header.remove()
```

- `ChildNode.remove()` - Node가 속한 트리에서 해당 Node를 제거

```js
const parent = document.querySelector('ul')
const child = document.querySelector('ul > li')

const removeChild = parent.removeChild(child)

console.log(removeChild)	// <li class="ssafy-location">서울</li> 삭제는 되었지만 값을 가지고 있음, 위치 바꿀때 이용 가능!!
```

- `Node.removeChild()` 

  - DOM에서 자식 Node를 제거하고 제거된 Node를 반환

  - Node는 인자로 들어가는 자식 Node의 부모 Node

<br>

### 🌈 DOM 속성 관련 메서드

```js
const header = document.querySelector('#location-header')

header.setAttribute('class', 'ssafy-location')
```

- `Element.setAttribute(name, value)`
  - 지정된 요소의 값을 설정
  - 속성이 이미 존재하면 값을 갱신, 존재하지 않으면 지정된 이름과 값으로 새 속성을 추가
  - header class에 ssafy-location 추가한다는 뜻



```js
const getAttr = document.querySelector('.ssafy-location')

getAttr.getAttribute('class')	// 'ssafy-location'
```

- `Element.getAttribute(attributeName)`
  - 해당 요소의 지정된 값(문자열)을 반환
  - 인자(attributeName)는 값을 얻고자 하는 속성의 이름

<br>



## 📮 결론

![image-20220427173042108](JavaScript%20-%20DOM%20%EC%A1%B0%EC%9E%91.assets/image-20220427173042108.png)