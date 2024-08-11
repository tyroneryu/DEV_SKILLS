# Start React
## Before React
### Vanila JS
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <span>Total Click : 0</span>
  <button id="btn">Click!</button>
</body>
  <script>
    let counter = 0
    const buttton = document.getElementById("btn")
    const span = document.querySelector("span")
    function handleClick() {
      counter = counter + 1
      span.innerText = `Total Click : ${counter}`
      console.log("I have been clicked")
    }
    button.addEventListener("click", handleClick)
  </script>
</html>
```
- 바닐라 JS로 위 코드를 구현하려면 고려해야 할 것이 너무도 많음

### React JS
- `script : src` 태그로 아래의 두 주소를 import
  - React: https://unpkg.com/react@17.0.2/umd/react.production.min.js
  - React dom: https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js
- react : HTML 생성, 앱이 interactive하게 만들어주는 라이브러리
- react-dom : react element들을 HTML body에 둘 수 있도록 함
- 아래의 react 내용은 실무에 쓰지 않고 어려운 방식
- 아래 방식을 이해하면 react 자체를 이해하기 쉬울 것
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="root"></div>
</body>
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script>
  const root = document.getElementById("root")
  const span = React.createElement("span", { id: "sexy-span", style: { color: "red" } }, "Hello I'm a span")
  ReactDOM.render(span, root)
</script>
</html>
```
- react는 script가 먼저, html이 나중에 생성
- react는 결과물인 HTML을 필요할 때마다 업데이트가 가능함

#### Vanila code -> React JS
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="root"></div>
</body>
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script>
  const root = document.getElementById("root")
  const h3 = React.createElement("h3", {
    onMouseEnter: () => console.log("Mouse enter")
  }, "Hello I'm a span")
  const span = React.createElement("span", null, "Hello I'm a span")
  const btn = React.createElement("button", {
    onClick: () => console.log("Hi")
  }, "Click me")
  const container = React.createElement("div", null, [h3, btn])
  ReactDOM.render(container, root)
</script>
</html>
```

### JSX
- JSX는 짱이다
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="root"></div>
</body>
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
  const root = document.getElementById("root")
  const Title = (
    <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
      hello I'm a title
    </h3>
  )
  const Button = (
    <button style={{backgroundColor: "tomato"}} onClick={() => console.log("im clicked")}>
      Click me
    </button>
  )
  const container = React.createElement("div", null, [Title, Button])
  ReactDOM.render(container, root)
</script>
</html>
```
- 크게 차이는 없지만 리액트에서 선호하는 방식이라고 함

- 컴포넌트는 첫글자는 무조건 대문자
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="root"></div>
</body>
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
  const root = document.getElementById("root")

  const Title = () => (
    <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
      hello I'm a title
    </h3>
  )

  const Button = () => (
    <button style={{backgroundColor: "tomato"}} onClick={() => console.log("im clicked")}>
      Click me
    </button>
  )

  const Container = () => (
    <div>
      <button>Hello</button>
      <Title />
      <Button />
    </div>
  )

  ReactDOM.render(<Container />, root)
</script>
</html>

```
