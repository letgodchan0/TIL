# 📒 JavaScript - 문자열 (String)

| 메서드   | 설명                                      | 비고                                         |
| -------- | ----------------------------------------- | -------------------------------------------- |
| includes | 특정 문자열의 존재여부를 참/거짓으로 반환 |                                              |
| split    | 문자열을 토큰 기준으로 나눈 배열 반환     | 인자가 없으면 기존 문자열을 배열에 담아 반환 |
| replace  | 해당 문자열을 대상 문자열로 교체하여 반환 | replaceAll                                   |
| trim     | 문자열의 좌우 공백 제거하여 반환          | trimStart, trimEnd                           |

<br>



### 💡includes

```javascript
constr str = 'a santa at nasa'

str.includes('santa')	// true
str.includes('asan')	// false
```

- 문자열에 value가 존재하는지 판별 후 참 또는 거짓 반환

<br>

### 💡 split

```javascript
const str = 'a coup'

str.split()		// ['a cup']
str.split('')	// ['a', ' ', 'c', 'u', 'p']
str.split(' ')	// ['a', 'cup']
```

- value가 없을 경우, 기존 문자열을 배열에 담아 반환
- value가 빈 문자열일 경우 각 문자로 나눈 배열을 반환
- value가 기타 문자열일 경우, 해당 문자열로 나눈 배열을 반환

<br>



### 💡 replace

```javascript
const str = 'a b c d'

str.replace(' ', '-')		// 'a-b c d'
str.replaceAll(' ', '-')	// 'a-b-c-d'
```

- `string.replace(from, to)`
  - 문자열에 from 값이 존재할 경우, 1개만 to 값으로 교체하여 반환
- `string.replaceAll(from, to)`
  - 문자열에 from 값이 존재할 경우, 모두 to 값으로 교체하여 반환

<br>



### 💡 trim

```javascript
const str = '	hello	'

str.trim()		// 'hello'
str.trimStart()	// 'hello	'
str.trimEnd()	// '	hello'
```

- `.trim()` : 문자열 시작과 끝의 모든 공백문자 (스페이스, 탭, 엔터 등)를 제거한 문자열 반환
- `.trimStart()` : 문자열 시작의 공백문자 (스페이스, 탭, 엔터 등)를 제거한 문자열 반환
- `.trimEnd()` : 문자열 끝의 공백문자 (스페이스, 탭, 엔터 등)를 제거한 문자열 반환



