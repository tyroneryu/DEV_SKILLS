# State
## setState
### Understanding State
- 비효율적인 방법으로 우선 생각해보자
- 변화가 생길때마다 reRnder를 해줘야 함
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
  let counter = 0
  function countUp() {
    counter = counter + 1
    render()
  }
  function render(){
    ReactDOM.render(<Container />, root)
  }
  const Container = () => (
    <div>
      <h3>Total clicks: {counter}</h3>
      <button onClick={countUp}>Click me</button>
    </div>
  )
  render()
</script>
</html>
```
- 우리가 집중해야 할 것은 데이터가 바뀔때마다 Container를 리렌더링해주는 것
- 리렌더링 해도 페이지 전체가 다시 생성되는 것이 아닌 변화가 있는 부분만 교체됨
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
  // let counter = 0
  // function countUp() {
  //   counter = counter + 1
  // }
  // function render(){
  //   ReactDOM.render(<App />, root) 
  // }
  function App() {
    const [counter, modifier] = React.useState(0)
    // const counter = data[0]
    // const modifier = data[1]
    return (
      <div>
        <h3>Total clicks: {counter}</h3>
        <button>Click me</button>
      </div>
    )
  }
  ReactDOM.render(<App />, root)
</script>
</html>
```
### modifier
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
  // let counter = 0
  // function countUp() {
  //   counter = counter + 1
  // }
  // function render(){
  //   ReactDOM.render(<App />, root) 
  // }
  function App() {
    const [counter, setCounter] = React.useState(0)
    // const counter = data[0]
    // const modifier = data[1]
    const onClick = () => {
      setCounter(counter + 1)
    }
    return (
      <div>
        <h3>Total clicks: {counter}</h3>
        <button onClick={onClick}>Click me</button>
      </div>
    )
  }
  ReactDOM.render(<App />, root)
</script>
</html>
```

## State Functions
- 현재 정보를 바탕으로 갱신하기
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
  // let counter = 0
  // function countUp() {
  //   counter = counter + 1
  // }
  // function render(){
  //   ReactDOM.render(<App />, root) 
  // }
  function App() {
    const [counter, setCounter] = React.useState(0)
    // const counter = data[0]
    // const modifier = data[1]
    const onClick = () => {
      setCounter((current) => current + 1)
    }
    return (
      <div>
        <h3>Total clicks: {counter}</h3>
        <button onClick={onClick}>Click me</button>
      </div>
    )
  }
  ReactDOM.render(<App />, root)
</script>
</html>
```

## Inputs and State
- 입력값을 받으려면 `onChange`
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
  
  function App() {
    const [minutes, setMinutes] = React.useState()
    const onChange = (event) => {
      // console.log(event.target.value)
      setMinutes(event.target.value)
    }
    return (
      <div>
        <h1 className="hi">Super Converter</h1>
        <label htmlFor="Minutes">Minutes: </label>
        <input value={minutes} id="minutes" placeholder="Minutes" type="number" onChange={onChange} />
        <h4>You want to convert {minutes}</h4>
        <label htmlFor="Hours">Hours: </label>
        <input placeholder="Hours" type="number" />
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```

## State practice
- 분을 시로 치환하는...
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
  
  function App() {
    const [minutes, setMinutes] = React.useState(0)
    const onChange = (event) => {
      // console.log(event.target.value)
      setMinutes(event.target.value)
    }
    const reset = () => setMinutes(0)
    return (
      <div>
        <div>
          <h1 className="hi">Super Converter</h1>
          <label htmlFor="Minutes">Minutes: </label>
          <input value={minutes} id="minutes" placeholder="Minutes" type="number" onChange={onChange} />
          <h4>You want to convert {minutes}</h4>
          <label htmlFor="Hours">Hours: </label>
          <input value={minutes / 60} id="hours" placeholder="Hours" type="number" />
        </div>
        <button onClick={reset}>Reset</button>
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```
- 분을 시로 뿐만 아니라 시를 분으로 치환도 가능하게 하기
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
  
  function App() {
    const [amount, setAmount] = React.useState(0)
    const [flipped, setFlipped] = React.useState(false)
    const onChange = (event) => {
      // console.log(event.target.value)
      setAmount(event.target.value)
    }
    const reset = () => setAmount(0)
    const onFlip = () => {
      reset()
      setFlipped((current) => !current)
    }
    return (
      <div>
        <div>
          <h1 className="hi">Super Converter</h1>
          <label htmlFor="Minutes">Minutes: </label>
          <input value={flipped ? amount * 60 : amount} id="minutes" placeholder="Minutes" type="number" onChange={onChange} disabled={flipped} />
          <h4>You want to convert {amount}</h4>
          <label htmlFor="Hours">Hours: </label>
          <input value={flipped ? amount : Math.round(amount / 60)} id="hours" placeholder="Hours" type="number" onChange={onChange} disabled={!flipped} />
        </div>
        <button onClick={reset}>Reset</button>
        <button onClick={onFlip}>{flipped ? "Turn back" : "Flip"}</button>
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```

## Practice
- 컴포넌트 단위로 쪼개 쓰기
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
  
  function MinutesToHours() {
    const [amount, setAmount] = React.useState(0)
    const [flipped, setFlipped] = React.useState(false)
    const onChange = (event) => {
      // console.log(event.target.value)
      setAmount(event.target.value)
    }
    const reset = () => setAmount(0)
    const onFlip = () => {
      reset()
      setFlipped((current) => !current)
    }
    return (
      <div>
        <div>
          <label htmlFor="Minutes">Minutes: </label>
          <input value={flipped ? amount * 60 : amount} id="minutes" placeholder="Minutes" type="number" onChange={onChange} disabled={flipped} />
          <label htmlFor="Hours">Hours: </label>
          <input value={flipped ? amount : Math.round(amount / 60)} id="hours" placeholder="Hours" type="number" onChange={onChange} disabled={!flipped} />
        </div>
        <button onClick={reset}>Reset</button>
        <button onClick={onFlip}>{flipped ? "Turn back" : "Flip"}</button>
      </div>
    )
  }

  function KmToMiles() {
    return <h3>KM 2 M</h3>
  }

  function App() {
    const [index, setIndex] = React.useState("xx")
    const onSelect = (event) => {
      setIndex(event.target.value)
    }
    return (
      <div>
        <h1>Super Converter</h1>
        <select value={index} onChange={onSelect}>
          <option value="xx">Select your units</option>
          <option value="0">Minutes & Hours</option>
          <option value="1">Km & Miles</option>
        </select>
        <hr />
        {index === "xx" ? "Please select your units" : null}
        {index === "0" ? <MinutesToHours /> : null}
        {index === "1" ? <KmToMiles /> : null}
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```
