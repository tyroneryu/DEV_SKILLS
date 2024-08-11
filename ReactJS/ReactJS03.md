# Props
## Props
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
  function Btn({text, big}) {
    return (
      <button 
        style={{
          backgroundColor: "tomato",
          color: "white",
          padding: "10px 20px",
          border: 0,
          borderRadius: 10,
          fontSize: big ? 18 : 16,
        }}>{text}</button>
    )
  }
  // function ConfirmBtn() {
  //   return <button>Confirm</button>
  // }
  function App() {
    return (
      <div>
        <Btn text="Save Changes" big={true} />
        <Btn text="Confirm" big={false} />
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```

## Memo
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
  function Btn({text, changeValue}) {
    return (
      <button 
        onClick={changeValue}
        style={{
          backgroundColor: "tomato",
          color: "white",
          padding: "10px 20px",
          border: 0,
          borderRadius: 10,
        }}>{text}</button>
    )
  }
  // function ConfirmBtn() {
  //   return <button>Confirm</button>
  // }
  const MemorizedBtn = React.memo(Btn)
  function App() {
    const [value, setValue] = React.useState("Save Changes")
    const changeValue = () => setValue("Revert Changes")
    return (
      <div>
        <MemorizedBtn text={value} changeValue={changeValue} />
        <MemorizedBtn text="Continue" />
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```

## Prop Types
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
<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
  function Btn({text, changeValue, fontSize}) {
    return (
      <button 
        onClick={changeValue}
        style={{
          backgroundColor: "tomato",
          color: "white",
          padding: "10px 20px",
          border: 0,
          borderRadius: 10,
          fontSize: fontSize,
        }}>{text}</button>
    )
    Btn.propTypes = {
      text: PropTypes.string.isRequired,
      fontSize: ProTypes.number,
    }
  }
  // function ConfirmBtn() {
  //   return <button>Confirm</button>
  // }
  const MemorizedBtn = React.memo(Btn)
  function App() {
    const [value, setValue] = React.useState("Save Changes")
    const changeValue = () => setValue("Revert Changes")
    return (
      <div>
        <MemorizedBtn text={value} changeValue={changeValue} />
        <MemorizedBtn text="Continue" fontSize={18} />
      </div>
    )
  }
  const root = document.getElementById("root")
  ReactDOM.render(<App />, root)
</script>
</html>
```
