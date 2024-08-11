# Practice Movie App
## Todo List
- 간단한 할일 목록 만들기~
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [toDo, setToDo] = useState("")
  const onChange = (event) => setToDo(event.target.value)
  const onSubmit = (event) => {
    event.preventDefault()
    if(toDo === "") {
      return
    }
    setToDo("")
  }
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input onChange={onChange} value={toDo} type="text" placeholder="Write your to do..." />
        <button>Add To Do</button>
      </form>
    </div>
  );
}

export default App;
```
- 할 일 목록을 나열해주면 끝~~
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [toDo, setToDo] = useState("")
  const [toDos, setToDos] = useState([])
  const onChange = (event) => setToDo(event.target.value)
  const onSubmit = (event) => {
    event.preventDefault()
    if(toDo === "") {
      return
    }
    setToDos((currentArray) => [toDo, ...currentArray])
    setToDo("")
  }
  return (
    <div>
      <h1>My To Dos ({toDos.length})</h1>
      <form onSubmit={onSubmit}>
        <input onChange={onChange} value={toDo} type="text" placeholder="Write your to do..." />
        <button>Add To Do</button>
      </form>
      <hr/>
      <ul>
        {toDos.map((item, index) => <li key={index}>{item}</li>)}
      </ul>
    </div>
  );
}

export default App;
```

## Coin Tracker
- 코인 가격 API 따와서 주루룩 나열해보자!
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [loading, setLoading] = useState(true)
  const [coins, setCoins] = useState([])
  useEffect(() => {
    fetch("https://api.coinpaprika.com/v1/tickers")
    .then((response) => response.json())
    .then((json) => {
      setCoins(json)
      setLoading(false)
    })
  }, [])
  return (
    <div>
      <h1>The Coins! ({coins.length})</h1>
      <hr/>
      {loading ? <strong>Loading...</strong> : null}
      <ul>
        {coins.map((coin) => <li>{coin.name} ({coin.symbol}): {coin.quotes.USD.price} USD</li>)}
      </ul>
    </div>
  );
}

export default App;
```

## Movie App
### Movie App 만들기 시작
```js
// import Button from "./Button"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [loading, setLoading] = useState(true)
  const [movies, setMovies] = useState([])
  const getMovies = async() => {
    const json = await (
      await fetch(
        'https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year'
      )
    ).json()
    setMovies(json.data.movies)
    setLoading(false)
  }
  useEffect(() => {
    getMovies()
  }, [])
  console.log(movies)
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map(movie => (
            <div key={movie.id}>
              <img src={movie.medium_cover_image} />
              <h2>{movie.title}</h2>
              <p>{movie.summary}</p>
              <ul>
                {movie.genres.map((g) => (
                  <li key={g}>{g}</li>
                ))}
              </ul>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

export default App;
```
- 위의 코드를 더 깔끔하게 바꿔보자
- component 사용
```js
// App.js

import Movie from "./Movie"
// import styles from "./App.module.css"
import { useState, useEffect } from "react"

function App() {
  const [loading, setLoading] = useState(true)
  const [movies, setMovies] = useState([])
  const getMovies = async() => {
    const json = await (
      await fetch(
        'https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year'
      )
    ).json()
    setMovies(json.data.movies)
    setLoading(false)
  }
  useEffect(() => {
    getMovies()
  }, [])
  console.log(movies)
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map(movie => 
            <Movie
              key={movie.id}
              coverImg={movie.medium_cover_image}
              title={movie.title}
              summary={movie.summary}
              genres={movie.genres}
            />
          )}
        </div>
      )}
    </div>
  );
}

export default App;
```
```js
// Movie.js

import PropTypes from "prop-types"
// import styles from "./Button.module.css"

function Movie({coverImg, title, summary, genres}) {
  return (
    <div>
      <img src={coverImg} alt={title} />
      <h2>{title}</h2>
      <p>{summary}</p>
      <ul>
        {genres.map((g) => (
          <li key={g}>{g}</li>
        ))}
      </ul>
    </div>
  )
}

Movie.propTypes = {
  coverImg: PropTypes.string.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie
```

### Router
- 일단 패키지 다운로드부터~~
- `npm i react-router-dom@5.3.0`

- Routes, Components 디렉토리를 만들어 정리
- App.js, Home.js 수정
```js
// App.js

import Home from "./routes/Home";
import { useState, useEffect } from "react"

function App() {
  return null
}

export default App;
```
```js
// Home.js

import Movie from "../components/Movie"
import { useState, useEffect } from "react"

function Home() {
  const [loading, setLoading] = useState(true)
  const [movies, setMovies] = useState([])
  const getMovies = async() => {
    const json = await (
      await fetch(
        'https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year'
      )
    ).json()
    setMovies(json.data.movies)
    setLoading(false)
  }
  useEffect(() => {
    getMovies()
  }, [])
  console.log(movies)
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map(movie => 
            <Movie
              key={movie.id}
              coverImg={movie.medium_cover_image}
              title={movie.title}
              summary={movie.summary}
              genres={movie.genres}
            />
          )}
        </div>
      )}
    </div>
  );
}

export default Home
```

### Route 사용법?!
```js
import Home from "./routes/Home";
import Detail from "./routes/Detail"
import { useState, useEffect } from "react"
import {
  BrowserRouter as Router,
  Switch,
  Route,
} from "react-router-dom"

function App() {
  return <Router>
    <Switch>
      <Route path="/movie">
        <Detail />
      </Route>
      <Route path="/">
        <Home />
      </Route>
    </Switch>
  </Router>
}

export default App;
```
- 링크 걸어주기
```js
import PropTypes from "prop-types"
import { Link } from "react-router-dom"
// import styles from "./Button.module.css"

function Movie({coverImg, title, summary, genres}) {
  return (
    <div>
      <img src={coverImg} alt={title} />
      <h2>
        <Link to="/movie">{title}</Link>
      </h2>
      <p>{summary}</p>
      <ul>
        {genres.map((g) => (
          <li key={g}>{g}</li>
        ))}
      </ul>
    </div>
  )
}

Movie.propTypes = {
  coverImg: PropTypes.string.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie
```
- 이제 영화들의 id를 따와서 각각 디테일 페이지 만들면 끝
```js
// Home.js

import Movie from "../components/Movie"
import { useState, useEffect } from "react"

function Home() {
  const [loading, setLoading] = useState(true)
  const [movies, setMovies] = useState([])
  const getMovies = async() => {
    const json = await (
      await fetch(
        'https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year'
      )
    ).json()
    setMovies(json.data.movies)
    setLoading(false)
  }
  useEffect(() => {
    getMovies()
  }, [])
  return (
    <div>
      {loading ? (
        <h1>Loading...</h1>
      ) : (
        <div>
          {movies.map(movie => 
            <Movie
              key={movie.id}
              id={movie.id}
              coverImg={movie.medium_cover_image}
              title={movie.title}
              summary={movie.summary}
              genres={movie.genres}
            />
          )}
        </div>
      )}
    </div>
  );
}

export default Home
```
```js
// Movie.js

import PropTypes from "prop-types"
import { Link } from "react-router-dom"
// import styles from "./Button.module.css"

function Movie({id, coverImg, title, summary, genres}) {
  return (
    <div>
      <img src={coverImg} alt={title} />
      <h2>
        <Link to={`/movie/${id}`}>{title}</Link>
      </h2>
      <p>{summary}</p>
      <ul>
        {genres.map((g) => (
          <li key={g}>{g}</li>
        ))}
      </ul>
    </div>
  )
}

Movie.propTypes = {
  id:PropTypes.number.isRequired,
  coverImg: PropTypes.string.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie
```
```js
// Detail.js

import { useParams } from "react-router-dom"
import { useEffect } from "react"

function Detail() {
  const {id} = useParams()
  const getMovie = async() => {
    const json = await(
      await fetch(`https://yts.mx/api/v2/movie_details.json?movie_id=${id}`)
    ).json()
    console.log(json)
  }
  useEffect(() => {
    getMovie()
  }, [])
  return <h1>Detail</h1>
}

export default Detail
```
