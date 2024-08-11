# JS
## 이벤트
- event: 무언가 일어났다는 신호, 사건
- 모든 DOM 요소는 이러한 event를 만들어 냄

### `event` object
- DOM에서 이벤트가 발생했을 때 생성되는 객체
- 이벤트 종류: `mouse`, `input`, `keyboard`, `touch`, ...
- DOM 요소는 event를 받고 받은 event(event handler 이벤트 처리기)를 처리할 수 있음

### event handler
- 이벤트가 발생했을 때 실행되는 함수
- 사용자의 행동에 어떻게 반응할 지를 JavaScript 코드로 표현한 것

#### `.addEventListener()`
- 대표적인 이벤트 핸들러 중 하나
- 특정 이벤트를 DOM 요소가 수신할 때마다 콜백 함수를 호출
- `EventTarget.addEventListener(type, handler)`
- `type`
    - 수신할 이벤트 이름
    - 문자열로 작성 (ex. `click`)
- `handler`
    - 발생한 이베느 객체를 수신하는 콜백 함수
    - 콜백 함수는 발생한 `event` object를 유일한 매개변수로 받음

#### `addEventListener` 활용
- 버튼을 클릭하면 버튼 요소 출력하기
- 버튼에 이벤트 처리기를 부착하여 클릭 이벤트가 발생하면 이벤트가 발생한 버튼 정보를 출력
![JS14](./asset/JS14.PNG)
- 요소에 `addEventListener`를 부착하게 되면 내부의 `this` 값은 대상 요소를 가리키게 됨
- event 객체의 `currentTarget` 속성 값과 동일
```js
// 1. 버튼 선택
const btn = document.querrySelector('#btn')

// 2. 콜백 함수
const detectClick = function(event) {
    console.log(event)                  // PointerEvent
    console.log(event.currentTarget)    // <button id="btn">버튼</button>
    console.log(this)                   // <button id="btn">버튼</button>
}

// 3. 버튼에 이벤트 핸들러를 부착
btn.addEventListener('click', detectClick)
```
- 발생한 이벤트를 나타내는 event 객체를 유일한 매개변수로 받음
- 반환 값 없음

### 버블링 개요
- `form` > `div` > `p` 형태의 중첩된 구조에 각각 이벤트 핸들러가 있을 때 만약 `<p>` 요소를 클릭하면 어떻게 될까
```html
<body>
  <form id="form">
    form
    <div id="div">
      div
      <p id="p">p</p>
    </div>
  </form>
</body>
```
```js
<script>
  const formElement = document.querySelector('#form')
  const divElement = document.querySelector('#div')
  const pElement = document.querySelector('#p')

  const clickHandler1 = function (event) {
    console.log('form이 클릭되었습니다.')
  }
  const clickHandler2 = function (event) {
    console.log('div가 클릭되었습니다.')
  }
  const clickHandler3 = function (event) {
    console.log('p가 클릭되었습니다.')
  }

  formElement.addEventListener('click', clickHandler1)
  divElement.addEventListener('click', clickHandler2)
  pElement.addEventListener('click', clickHandler3)
</script>
```
- `<p>` 요소만 클릭했음에도 모든 핸들러가 동작함
![JS15](./asset/JS15.PNG)

### 버블링 (Bubbling)
- 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작하는 현상
- 가장 최상단의 조상 요소 (`document`)를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작
- 이벤트가 제일 깊은 곳에 있는 요소에서 시작해 부모 요소를 거슬러 올라가며 발생하는 것이 마치 물속 거품과 닮았기 때문
- 가장 안쪽의 `<p>` 요소를 클릭하면 `p` -> `div` -> `form` 순서로 3개의 이벤트 핸들러가 모두 동작했던 것

#### `currentTarget` & `target` 속성
- `currentTarget` 속성
    - 현재 요소
    - 항상 이벤트 핸들러가 연결된 요소만을 참조하는 속성
    - `this`와 같음
- `target` 속성
    - 이벤트가 발생한 가장 안쪽의 요소(target)를 참조하는 속성
    - 실제 이벤트가 시작된 요소
    - 버블링이 진행되어도 변함이 없음

#### 예시
- 세 요소 중 가장 최상위 요소인 `outerouter` 요소에만 핸들러가 부착
- 각 요소를 클릭했을 때 event 의 `target`과 `currentTarget`의 차이 비교

![JS16](./asset/JS16.PNG)
- `currentTarget`: 핸들러가 연결된 `outerouter` 요소만을 가리킴
- `target`: 실제 이벤트가 발생하는 요소를 가리킴
- 핸들러는 `outerouter`에만 할당되어 있지만 하위 요소 `outer`와 `inner`를 클릭해도 해당 핸들러가 동작함
- 클릭 이벤트가 어디서 발생했든 상관없이 `outerouter`까지 이벤트가 버블링 되어 핸들러를 실행 시킴
```html
<style>
  #outerouter {
    width: 300px;
    height: 300px;
    background-color: yellowgreen;
  }

  #outer {
    width: 200px;
    height: 200px;
    background-color: crimson;
  }

  #inner {
    width: 100px;
    height: 100px;
    background-color: skyblue;
  }
</style>

<body>
  <div id="outerouter">
    outerouter
    <div id="outer">
      outer
      <div id="inner">inner</div>
    </div>
  </div>

  <script>
    const outerOuterElement = document.querySelector('#outerouter')
    const outerElement = document.querySelector('#outer')
    const innerElement = document.querySelector('#inner')

    const clickHandler = function (event) {
      console.log('currentTarget:', event.currentTarget.id)
      console.log('target:', event.target.id)
    }

    outerOuterElement.addEventListener('click', clickHandler)
  </script>
</body>
```

### 캡처링(capturing)
- 이벤트가 하위 요소로 전파되는 단계(버블링과 반대)
![JS17](./asset/JS17.PNG)

#### 캡처링과 버블링
- `table` 안에 `td`를 클릭하면 이벤트는 최상위 요소부터 아래로 전파
- 실제 이벤트가 발생한 지점(`event.target`)에서 실행된 후 다시 위로 전파
- 이 과정에서 상위 요소에 할당된 이벤트 핸들러가 호출되는 것
- 캡처링은 실제 다로는 경우가 거의 없으므로 버블링에 집중하기

#### 버블링이 필요한 이유
- 만약 각자 다른 동작을 수행하는 버튼이 여러 개가 있다고 가정
- 그렇다면 각 버튼마다 서로 다른 이벤트 핸들러를 할당해야 하는가
- 각 버튼의 공통 조상인 `div` 요소에 이벤트 핸들러 단 하나만 할당하기
- 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당하면 여러 요소를 한꺼번에 다룰 수 있음
- 공통 조상에 할당한 핸들러에서 `event.target`을 이용하면 실제 어떤 버튼에서 이벤트가 발생했는지 알 수 있기 때문
```js
const divTag = document.querySelector('div')

divTag.addEventListener('click', function(event) {
    console.log(event, target)
})
```

### `event handler`
#### 1. click 이벤트 실습
- 버튼을 클릭하면 숫자를 1씩 증가
![JS18](./asset/JS18.PNG)
```html
<body>
  <button id="btn">버튼</button>
  <p>클릭횟수 : <span id="counter">0</span></p>

  <script>
    // 1. 초기값 할당
    let countNumber = 0

    // 2. 버튼 요소 선택
    const btn = document.querySelector('#btn')

    // 3. 콜백 함수(버튼에 클릭 이벤트가 발생할 때마다 실행할 코드)
    const clickHandler = function() {
      // 3.1. 초기값 += 1
      countNumber += 1
      // 3.2. p 요소를 선택
      const spanTag = document.querySelector('#counter')
      // 3.3. p 요소의 컨텐츠를 1 증가한 초기값으로 설정
      spanTag.textContent = countNumber
    }

    // 4. 버튼에 이벤트 핸들러 부착(클릭 이벤트)
    btn.addEventListener('click', clickHandler)
  </script>
</body>
```

#### 2. input 이벤트 실습
- 사용자의 입력 값을 실시간으로 출력하기
![JS19](./asset/JS19.PNG)
```html
<body>
  <input type="text" id="text-input">
  <p></p>

  <script>
    // 1. input 요소 선택
    const inputTag = document.querySelector('#text-input')

    // 2. p 요소 선택
    const pTag = document.querySelector('p')

    // 3. 콜백 함수 (input 요소에 input 이벤트가 발생할 때마다 실행할 코드)
    const inputHandler = function(event) {
      // 3.1. 작성하는 데이터가 어디에 누적되고 있는지 찾기
      // console.log(event.currentTarget)
      console.log(event.currentTarget.value)
      // 3.2. p 요소의 컨텐츠에 작성하는 데이터를 추가
      pTag.textContent = event.currentTarget.value
    }

    // 4. input 요소에 이벤트 핸들러 부착(input 이벤트)
    inputTag.addEventListener('input', inputHandler)
  </script>
</body>
```

### `currnetTarget` 주의 사항
- `console.log()`로 event 객체를 출력할 경우 `currentTarget` 키의 값은 `null`을 가짐
- `currentTarget`은 이벤트가 처리되는 동안에만 사용할 수 있기 때문
- 대신 `console.log(event.currentTarget)`을 사용하여 콘솔에서 확인 가능
- `currentTarget` 이후의 속성 값들은 `target`을 참고해서 사용하기

#### 3. click & input 이벤트 실습
- 사용자의 입력 값을 실시간으로 출력
- `+` 버튼을 클릭하면 출력한 값의 CSS 스타일을 변경하기
![JS20](./asset/JS20.PNG)
```html
<body>
  <h1></h1>
  <button id="btn">클릭</button>
  <input type="text" id="text-input">

  <script>
    // input 구현
    const inputTag = document.querySelector('#text-input')
    const h1Tag = document.querySelector('h1')

    const inputHandler = function(event) {
      h1Tag.textContent = event.currentTarget.value
    }
    inputTag.addEventListener('input', inputHandler)

    // click 구현
    const btn = document.querySelector('#btn')

    const clickHandler = function() {
      // 1. add 방법
      // h1Tag.classList.add('blue')
      // 2. toggle 방법
      // h1Tag.classList.toggle('blue')
      // 3. if 방법
      if (h1Tag.classList.value) {
        h1Tag.classList.remove('blue')
      } else {
        h1Tag.classList.add('blue')
      }
    }

    btn.addEventListener('click', clickHandler)
  </script>
</body>
```

#### 4. todo 실습
![JS21](./asset/JS21.PNG)
```html
<body>
  <input type="text" class="input-text">
  <button id="btn">+</button>
  <ul></ul>

  <script>
    // 1. 필요한 요소 선택
    const inputTag = document.querySelector('.input-text')
    const btn = document.querySelector('#btn')
    const ulTag = document.querySelector('ul')

    const addTodo = function(event) {
      // 2.1. 사용자 입력 데이터 저장
      const inputData = inputTag.value
      // 2.2. 데이터를 저장할 li 요소를 생성
      const liTag = document.createElement('li')
      // 2.3. li 요소 컨텐츠에 데이터 입력
      liTag.textContent = inputData
      // 2.4. li 요소를 부모 ul 요소의 자식 요소로 추가
      ulTag.appendChild(liTag)
      // 2.5. todo 추가 후 input의 입력 데이터는 초기화
      inputTag.value = ''
    }
    btn.addEventListener('click', addTodo)
  </script>
</body>
```
- todo 추가 기능 구현
1. 빈 문자열 입력 방지
2. 입력이 없을 경우 경고 대화상자를 띄움
```html
<body>
  <input type="text" class="input-text">
  <button id="btn">+</button>
  <ul></ul>

  <script>
    // 1. 필요한 요소 선택
    const inputTag = document.querySelector('.input-text')
    const btn = document.querySelector('#btn')
    const ulTag = document.querySelector('ul')

    const addTodo = function(event) {
      // 2.1. 사용자 입력 데이터 저장
      const inputData = inputTag.value
      if (inputData.trim()) {
        // 2.2. 데이터를 저장할 li 요소를 생성
        const liTag = document.createElement('li')
        // 2.3. li 요소 컨텐츠에 데이터 입력
        liTag.textContent = inputData
        // 2.4. li 요소를 부모 ul 요소의 자식 요소로 추가
        ulTag.appendChild(liTag)
        // 2.5. todo 추가 후 input의 입력 데이터는 초기화
        inputTag.value = ''
      } else {
        alert('할 일을 입력하세요..')
      }
    }
    btn.addEventListener('click', addTodo)
  </script>
</body>
```

#### 5. 로또 번호 생성기 실습
![JS22](./asset/JS22.PNG)
```html
<body>
  <h1>로또 추천 번호</h1>
  <button id="btn">행운 번호 받기</button>
  <div></div>

  <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
  <script>
    // 1. 필요한 요소 선택
    const h1Tag = document.querySelector('h1')
    const btn = document.querySelector('#btn')
    const divTag = document.querySelector('div')

    // 2. 로또 번호를 생성하는 함수
    const getLottery = function(event) {
      // 2.1. 1부터 45까지의 값이 필요
      const numbers = _.range(1, 46)
      // 2.2. 45개의 요소가 있는 배열에서 6개 번호 추출
      const sixNumbers = _.sampleSize(numbers, 6)
      // 2.5. 6개의 li 요소를 담을 ul 요소 생성
      const ulTag = document.createElement('ul')
      // 2.3. 추출한 번호 배열을 반복하면서 li 요소를 생성
      sixNumbers.forEach((number) => {
        // 2.4. 번호를 담을 li 요소 생성 후 입력
        const liTag = document.createElement('li')
        liTag.textContent = number
        // 2.6. 만들어진 li를 ul 요소에 추가
        ulTag.appendChild(liTag)
      })
      // 2.7. 완성한 ul 요소를 div 요소에 추가
      divTag.appendChild(ulTag)
    }
    // 3. 버튼 요소에 이벤트 핸들러를 부착
    btn.addEventListener('click', getLottery)
  </script>
</body>
```

### lodash
- 모듈성, 성능 및 추가 기능을 제공하는 JavaScript 유틸리티 라이브러리
- array, object 등 자료구조를 다룰 때 사용하는 유요하고 간편한 함수들을 제공

### 이벤트 기본 동작 취소하기
- HTML의 각 요소가 기본적으로 가지고 있는 이벤트가 때로는 방해가 되는 경우가 있어 이벤트의 기본 동작을 취소할 필요가 있음
- 예시
    - `form` 요소의 제출 이벤트를 취소하여 페이지 새로고침을 막을 수 있음
    - `a` 요소를 클릭할 때 페이지 이동을 막고 추가 로직을 수행할 수 있음

#### `.preventDefault()`
- 해당 이벤트에 대한 기본 동작을 실행하지 않도록 지정

#### 이벤트 동작 취소 실습
- `copy` 이벤트 동작 취소
- 콘텐트를 복사하는 것을 방지
![JS23](./asset/JS23.PNG)
- `form` 제출 시 페이지 새로고침 동작 취소
- `form` 요소의 `submit` 동작(`action` 값으로 요청)을 취소 시킴
```html
<body>
  <h1>중요한 내용</h1>

  <form id="my-form">
    <input type="text" name="username">
    <button type="submit">Submit</button>
  </form>

  <script>
    // 1. copy 이벤트 취소
    const h1Tag = document.querySelector('h1')

    h1Tag.addEventListener('copy', function(event) {
      console.log(event)
      event.preventDefault()
      alert('복사할 수 없습니다.')
    })
    // 2. submit 이벤트 취소
    const formTag = document.querySelector('#my-form')
    const handleSubmit = function(event) {
      event.preventDefault()
    }
    formTag.addEventListener('submit', handleSubmit)
  </script>
</body>
```

### 참고
#### `addEventListener`에서의 화살표 함수 주의 사항
- 화살표 함수는 자신만의 `this`를 가지지 않기 때문에 자신을 포함하고 있는 함수의 `this`를 상속받음
- `this`를 사용해야 하는 경우 `addEventListener`에서는 일반 함수로 사용
