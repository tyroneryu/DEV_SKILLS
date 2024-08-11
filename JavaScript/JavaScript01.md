# JavaScript
## DOM
- 웹 브라우저에서의 JS: 웹 페이지의 동적인 기능을 구현
#### JS 실행 환경 종류
1. HTML `script` 태그
2. js 확장자 파일
3. 브라우저 Console

### DOM
- The Document Object Model
- 웹 페이지(Document)를 구조화된 객체로 제공하여 프로그래밍 언어가 페이지 구조에 접근할 수 있는 방법을 제공
- 문서 구조, 스타일, 내용 등을 변경할 수 있도록 함
- 핵심: 문서의 요소들을 객체로 제공하여 다른 프로그래밍 언어에서 접근하고 조작할 수 있는 방법을 제공하는 API

#### DOM API
- 다른 프로그래밍 언어가 웹 페이지에 접근 및 조작할 수 있도록 페이지 요소들을 객체 형태로 제공하며 이에 따른 메서드 또한 제공
![JS01](./asset/JS01.PNG)

#### DOM 특징
- DOM에서 모든 요소, 특성, 텍스트는 하나의 객체
- 모두 document 객체의 하위 객체로 구성됨
![JS02](./asset/JS02.PNG)

#### DOM tree
- 브라우저는 HTML 문서를 해석하여 DOM tree라는 객체 트리로 구조화
- 객체 간 상속 구조가 존재
![JS03](./asset/JS03.PNG)

#### 브라우저가 웹 페이지를 불러오는 과정
- 웹 페이지는 웹 브라우저를 통해 해석되어 웹 브라우저 화면에 나타남
![JS04](./asset/JS04.PNG)

### `document` 객체
- 웹 페이지 객체
- DOM Tree의 진입점
- 페이지를 구성하는 모든 객체 요소를 포함

#### `document` 객체 예시
- HTML의 `<title>` 변경하기
![JS05](./asset/JS05.PNG)

## DOM 선택
### DOM 조작 시 기억해야 할 것
- 웹 페이지를 동적으로 만들기 == 웹 페이지를 조작하기
- 조작 순서
1. 조작하고자 하는 요소를 선택(또는 탐색)
2. 선택된 요소의 콘텐츠 또는 속성을 조작

### 선택 메서드
- `document.querySelector()`: 요소 한개 선택
- `document.querySelectorAll()`: 요소 여러개 선택

#### `document.querySelector(selector)`
- 제공한 선택자와 일치하는 element 한 개 선택
- 제공한 CSS selector를 만족하는 첫번째 element 객체를 반환 
- 없다면 `null` 반환

#### `document.querySelectorAll(selector)`
- 제공한 선택자와 일치하는 여러 element를 선택
- 제공한 CSS selector를 만족하는 NodeList를 반환

#### DOM 선택 실습
```html
<!-- select.html -->

<body>
  <h1 class="heading">DOM 선택</h1>
  <a href="https://www.google.com/">google</a>
  <p class="content">content1</p>
  <p class="content">content2</p>
  <p class="content">content3</p>
  <ul>
    <li>list1</li>
    <li>list2</li>
  </ul>
  <script>
    console.log(document.querySelector('.heading'))
    console.log(document.querySelector('.content'))
    console.log(document.querySelectorAll('.content'))
    console.log(document.querySelectorAll('ul > li'))
  </script>
</body>
```
![JS06](./asset/JS06.PNG)

### DOM 조작
1. 속성(attribute) 조작
    - 클래스 속성 조작
    - 일반 속성 조작
2. HTML 콘텐츠 조작
3. DOM 요소 조작
4. 스타일 조작

### 속성 조작
#### 클래스 속성 조작
- `classList` property
- 요소의 클래스 목록을 DOMTokenList(유사 배열) 형태로 반환

#### 1. classList 메서드
- `element.classList.add()`: 지정한 클래스 값을 추가
- `element.classList.remove()`: 지정한 클래스 값을 제거
- `element.classList.toggle()`: 클래스가 존재한다면 제거하고 false 반환
    - 존재하지 않으면 클래스를 추가하고 true를 반환

#### 클래스 속성 조작 실습
- `add()`와 `remove()` 메서드를 사용해 지정한 클래스 값을 추가 혹은 제거
```html
<!-- element-manipulation.html -->

<style>
  .red {
    color: crimson;
  }
</style>
```
![JS07](./asset/JS07.PNG)
```html
<!-- element-manipulation.html -->

<script>
  const h1Tag = document.querySelector('.heading')
  console.log(h1Tag.classList)

  h1Tag.classList.add('red')
  console.log(h1Tag.classList)

  h1Tag.classList.remove('red')
  console.log(h1Tag.classList)

  h1Tag.classList.toggle('red')
  console.log(h1Tag.classList)
</script>
```

#### 2. 일반 속성 조작 메서드
- `Element.getAttribute()`: 해당 요소에 지정된 값을 반환(조회)
- `Element.setAttribute(name, value)`: 지정된 요소의 속성 값을 설정
    - 속성이 이미 있으면 기존 값을 갱신 (그렇지 않으면 지정된 이름과 값으로 새 속성이 추가)
- `Element.removeAttribute()`: 요소에서 지정된 이름을 가진 속성 제거

#### 일반 속성 조작 실습
```html
<!-- element-manipulation.html -->

<script>
  const aTag = document.querySelector('a')
  console.log(aTag.getAttribute('href'))

  aTag.setAttribute('href', 'https://www.naver.com/')
  console.log(aTag.getAttribute('href'))

  aTag.removeAttribute('href')
  console.log(aTag.getAttribute('href'))
</script>
```
![JS08](./asset/JS08.PNG)

### HTML 콘텐츠 조작
- `twxtContent` property
- 요소의 텍스트 콘텐츠를 표현
- `<p> lorem </p>`

#### HTML 콘텐츠 조작 실습
```html
<!-- contents-manipulation.html -->

<script>
  const h1Tag = document.querySelector('.heading')
  console.log(h1Tag.textContent)

  h1Tag.textContent = '내용 수정'
  console.log(h1Tag.textContent)
</script>
```
![JS09](./asset/JS09.PNG)

### DOM 요소 조작
#### DOM 요소 조작 메서드
- `document.createElement(tagName)`: 작성한 tagName의 HTML 요소를 생성하여 반환
- `Node.appendChild()`: 한 Node를 특정 부모 Node의 자식 NodeList 중 마지막 자식으로 삽입
    - 추가된 Node 객체를 반환
- `Node.removeChild()`: DOM에서 자식 Node를 제거
    - 제거된 Node를 반환

#### DOM 요소 조작 실습
```html
<!-- dom-manipulation.html -->

<script>
  // 생성
  const h1Tag = document.createElement('h1')
  h1Tag.textContent = '제목'
  console.log(h1Tag)
  // 추가
  const divTag = document.querySelector('div')
  divTag.appendChild(h1Tag)
  console.log(divTag)
  // 삭제
  divTag.removeChild(h1Tag)
</script>
```
![JS10](./asset/JS10.PNG)

### style 조작
- `style` property
- 해당 요소의 모든 style 속성 목록을 포함하는 속성

#### style 조작 실습
```html
<!-- style-property.html -->

<script>
  const pTag = document.querySelector('p')

  pTag.style.color = 'crimson'
  pTag.style.fontSize = '2rem'
  pTag.style.border = '1px solid black'
  console.log(pTag.style)
</script>
```
![JS11](./asset/JS11.PNG)

### 참고
#### Node
- DOM의 기본 구성 단위
- DOM 트리의 각 부분은 Node라는 객체로 표현됨
    - Document Node -> HTML 문서 전체를 나타내는 노드
    - Element Node -> HTML 요소를 나타내는 노드 (예: `<p>`)
    - Text Node -> HTMl 텍스트 (Element Node 내의 텍스트 컨텐츠를 나타냄)
    - Attribute Node -> HTML 요소의 속성을 나타내는 노드

#### NodeList
- DOM 메서드를 사용해 선택한 Node 목록
- 배열과 유사한 구조를 가짐
- Index로만 각 항목에 접근 가능
- JavaScript의 배열 메서드 사용 가능
- `querySelectorAll()`에 의해 반환되는 NodeList는 DOM의 변경사항을 실시간으로 반영하지 않음
- DOM이 나중에 변경되더라도 이전에 이미 선택한 NodeList 값은 변하지 않음

#### Element
- Node의 하위 유형
- ELement는 DOM 트리에서 HTML 요소를 나타내는 특별한 유형의 Node
- 예를 들어 `<p>`, `<div>`, `<span>`, `<body>` 등의 HTML 태그들이 Element 노드를 생성
- Node의 속성과 메서드를 모두 가지고 있으며 추가적으로 요소 특화된 기능 (예: `className`, `innerHTML`, `id`, 등)을 가지고 있음
- 모든 Element는 Node이지만, 모든 Node가 Element인 것은 아님

#### DOM 속성 확인 Tip
- 개발자도구 - Elements - Properties
- 해당 요소의 모든 DOM 속성 확인 가능
![JS12](./asset/JS12.PNG)

#### Parsing
- 구문 분석, 해석
- 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정
![JS13](./asset/JS13.PNG)
