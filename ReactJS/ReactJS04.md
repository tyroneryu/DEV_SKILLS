# Create React App
## Introduction
- script에 url 다는 것과는 큰 차이가..
- create app 하는 법
```
npx create-react-app my-app
cd my-app
npm start
```
- 와! 프로젝트 생성된다!

## Tour of CRA
```js
// Button.js

function Button({text}) {
  return <button>{text}</button>
}
export default Button
```
```js
// App.js

import Button from "./Button"

function App() {
  return (
    <div>
      <h1>Welcome back!</h1>
      <Button />
    </div>
  );
}

export default App;
```
- props를 인스톨하자!
- `npm i prop-types`

- styles.css를 사용하면 적용은 쉽지만 세세하게 조정이 안됨
```css
/* styles.css */

button {
  color: white;
  background-color: tomato;
}
```
- 따라서 사용하지 않을 것임
- 각각에 style을 넣어주면 적용이 되기는 하지만 귀찮아
```js
// button.js

import PropTypes from "prop-types"

function Button({text}) {
  return <button style={{
    backgroundColor: "tomato",
    color: "white",
  }}>{text}</button>
}

Button.propTypes = {
  text: PropTypes.string.isRequired,
}

export default Button
```
- 따라서 css 모듈들을 각각 만들어줘
- styles.css 의 이름을 Button.module.css로 바꾸고 Button.js에 import
```js
// Button.js

import PropTypes from "prop-types"
import styles from "./Button.module.css"

function Button({text}) {
  return <button className={styles.btn}>{text}</button>
}

Button.propTypes = {
  text: PropTypes.string.isRequired,
}

export default Button
```

## State
```js
import Button from "./Button"
import styles from "./App.module.css"
import { useState } from "react"

function App() {
  const [counter, setValue] = useState(0)
  const onClick = () => setValue((prev) => prev + 1)
  return (
    <div>
      <h1 className={styles.title}>{counter}</h1>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}

export default App;
```

### useEffect
- component의 첫번째 render시점에 useEffect가 우리의 함수를 호출함
```js
import Button from "./Button"
import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [counter, setValue] = useState(0)
  const [keyword, setKeyword] = useState("")
  const onClick = () => setValue((prev) => prev + 1)
  const onChange = (event) => setKeyword(event.target.value)
  const iRunOnlyOnce = () => {}
  useEffect(iRunOnlyOnce, [])
  return (
    <div>
      <input value={keyword} onChange={onChange} type="text" placeholder="Search here..." />
      <h1 className={styles.title}>{counter}</h1>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}

export default App;
```

## Deps
- useEffect를 통해 내가 원하는 타겟이 변화가 있을때만 재 호출 되게 만들 수 있음
```js
// import Button from "./Button"
import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [counter, setValue] = useState(0)
  const [keyword, setKeyword] = useState("")
  const onClick = () => setValue((prev) => prev + 1)
  const onChange = (event) => setKeyword(event.target.value)
  useEffect(() => {
    console.log("I run only once.")
  }, [])
  useEffect(() => {
    console.log("I run when 'keyword' changes.")
  }, [keyword])
  useEffect(() => {
    console.log("I run when 'counter' changes.")
  }, [counter])
  useEffect(() => {
    if (keyword !== "") {
      console.log("SEARCH FOR", keyword)
    }
  }, [keyword])
  return (
    <div>
      <input value={keyword} onChange={onChange} type="text" placeholder="Search here..." />
      <h1 className={styles.title}>{counter}</h1>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}

export default App;
```

## Cleanup Function
- component를 숨기고 표시하고를 조절 가능
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function Hello(){
  useEffect(() => {
    console.log("I'm here")
  }, [])
  return <h1>Hello</h1>
}

function App() {
  const [showing, setShowing] = useState(false)
  const onClick = () => setShowing((prev) => !prev)
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```
- 컴포넌트가 사라질때 무언가를 할당할 수 있어서 cleanup function이라고 불림
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function Hello(){
  useEffect(() => {
    console.log("Created :)")
    return () => console.log("Destroyed :(")
  }, [])
  return <h1>Hello</h1>
}

function App() {
  const [showing, setShowing] = useState(false)
  const onClick = () => setShowing((prev) => !prev)
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```
- 아래의 방식도 가능함
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function Hello(){
  function byFn() {
    console.log("Bye :(")
  }
  function hiFn() {
    console.log("Created :)")
    return byFn
  }
  useEffect(hiFn, [])
  return <h1>Hello</h1>
}

function App() {
  const [showing, setShowing] = useState(false)
  const onClick = () => setShowing((prev) => !prev)
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```
- 아래와 같은 방법을 선호한다!
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function Hello(){
  useEffect(() => {
    console.log("hi :)")
    return () => console.log("bye :(")
  }, [])
  return <h1>Hello</h1>
}

function App() {
  const [showing, setShowing] = useState(false)
  const onClick = () => setShowing((prev) => !prev)
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```
