## Routing

<br>

### Routing이 뭘까?

라우팅은 어플리케이션의 엔드포인트가(URIs) 클라이언트 요청에 어떻게 응답하는지에 관한 것을 말한다.

Express에서는 HTTP 메서드에 상응하는 Express의 app 객체의 메서드를 사용하여 라우팅을 정의한다.  예를 들어, app.get()은 GET 요청을 다루고, app.post는 POST 요청을 다루는 식이다. Express에서는 요청 하나하나에 대해서 대응할 수 있을 뿐만 아니라, app.all()을 사용하여 모든 HTTP 메서드를 다룰 수 있다. 또한 app.use()를 사용하여 미들웨어를 콜백 함수로 지정할 수 있다.

이 라우팅 메서드들은 어플리케이션이 특정 루트(엔드포인트) 및 HTTP 메서드 요청을 받을 때 실행할 콜백 함수(핸들러 함수)를 지정할 수 있다. 다시 말해서, 이 애플리케이션은 특정한 경로(들)와 메서드(들)에 일치하는 요청을 "듣고" 있는 것이다. 만약 일치하게 되면 콜백 함수를 호출하는 방식이다.

사실 라우팅 메서드는 하나 이상의 콜백 함수를 인자로 가질 수 있다. 여러 개의 콜백 함수를 가진 경우, next를 콜백 함수의 인자로 전달해 주는 것이 중요하다. 그래서 한 콜백 함수 내에서 동작이 끝나면 next()를 호출하여 다음 콜백으로 제어를 전달하게 된다.

다음의 코드는 가장 기본적인 라우트를 나타내고 있다.

```javascript
var express = require('express')
var app = express()

// 홈페이지에서 GET 요청이 발생하는 경우 "hello world"로 반응하게 된다. 
app.get('/', function (req, res) {
  res.send('hello world')
})
```

<br>

### Route methods

라우트 메서드는 HTTP 메서드 중 하나에 기인하며, express 클래스의 인스턴스에 포함된다.

다음 코드는 앱의 경로에 대한 GET 및 POST 메서드에 대해 정의 된 경로의 예이다.

```javascript
// GET 메서드 루트
app.get('/', function (req, res) {
  res.send('GET request to the homepage')
})

// POST 메서드 루트
app.post('/', function (req, res) {
  res.send('POST request to the homepage')
})
```

Express는 get, post 등등의 모든 HTTP 요청 메서드에 대응하는 메서드를 지원한다.

또 특별한 라우팅 메서드가 있는데, 바로 app.all() 메서드이다. 이 app.all() 메서드는 모든 HTTP 요청 메서드에 대한 경로에서 미들웨어 함수를 로드하는데 사용된다. 예를 들어, 다음 핸들러는 GET, POST, PUT, DELETE 또는 http 모듈에서 지원하는 다른 모든 HTTP 요청 메서드에 대해서 "/secret" 경로에 대한 요청에 대해 실행된다.

```javascript
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next() // 다음 핸들러에게 제어를 전달한다.
})
```

<br>

### Route paths

라우트 경로는 요청 메서드와 함께 요청이 생성 될 수 있는 엔드포인트를 정의한다. 라우트 경로는 문자열, 문자열 패턴 혹은 정규 표현식으로 표현할 수 있다.

문자 ?, +, *, 그리고 ()는 정규 표현식의 하위 집합이다. 하이픈 (-)과 점 (.)은 문자그대로 문자열 기반 경로로 해석된다.

만일 경로 문자열에 달러 문자($)를 사용해야하는 경우, 달러 문자를 ([ and ])로 묶어 제외(escape)하도록 해야한다. 예를 들어, "data/달러 문자book"의 요청에 대한 경로 문자열은 "data/([ \달러 문자 ])book"이 되어야 한다.

#### Route parameters

라우트 매개 변수는 URL에서 해당 위치에 지정된 값을 포착하는데 사용되는, 이름이 지정된 URL 세그먼트이다. 포착된 값은 req.params에 채워지며, 경로에 지정된 라우트 매개 변수의 이름이 값들에 대한 해당 key로써 지정된다.

``` terminal
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

라우트 매개 변수와 함께 라우트를 정의하려면, 아래 처럼 라우트의 경로 안에 라우트 매개 변수를 지정하면 된다.

```javascript
app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
})
```

하이픈 (-)과 점 (.)은 문자 그대로 해석되므로 라우트 매개 변수와 함께 유용하게 사용할 수 있다.

```terminal
Route path: /flights/:from-:to
Request URL: http://localhost:3000/flights/LAX-SFO
req.params: { "from": "LAX", "to": "SFO" }
```

```terminal
Route path: /plantae/:genus.:species
Request URL: http://localhost:3000/plantae/Prunus.persica
req.params: { "genus": "Prunus", "species": "persica" }
```

라우트 매개 변수와 일치할 수 있는 정확한 문자열을 보다 잘 제어하려면 괄호로 정규 표현식을 추가할 수 있다.

```terminal
Route path: /user/:userId(\d+)
Request URL: http://localhost:3000/user/42
req.params: {"userId": "42"}
```

<br>

### Route handlers

요청을 다루기 위해서 미들웨어처럼 동작하는 콜백 함수를 라우트 메서드에 여러개 전달할 수 있다. 유일한 예외는 이러한 콜백이 next('route')를 호출하여 나머지 라우트 콜백을 무시할 수 있다는 점이다. 이 매커니즘을 사용하여 라우트에 사전 조건을 적용한 다음, 현재 경로를 진행할 이유가 없는 경우 다음 경로로 제어를 전달할 수 있다.

라우트 핸들러는 아래의 예시들처럼 함수의 형태나, 함수가 담긴 배열, 혹은 이 두 방법을 조합하여 전달할 수 있다.

단일 콜백 함수로 다음과 같이 라우트를 다룰 수 있다.

```javascript
app.get('/example/a', function (req, res) {
  res.send('Hello from A!')
})
```

하나 이상의 콜백 함수로 라우트를 다룰 수 있다. (next 객체를 지정해야 하는 점을 잊지말자!)

```javascript
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from B!')
})
```

콜백 함수들이 담긴 배열로 라우트를 다룰 수도 있다.

```javascript
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

var cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])
```

독립적인 함수와 함수들이 담긴 배열로 라우트를 다룰 수도 있다.

```javascript
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from D!')
})
```

<br>

### Response methods

response 객체(res)의 메서드들은 클라이언트에 응답을 전달할 수 있고, 요청-응답 사이클을 종료한다. 라우트 핸들러에서 이 메서드 중 어느 것도 호출되지 않으면 클라이언트 요청이 정지 된 상태로 남아있게 된다.

|                            Method                            |                         Description                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| [res.download()](https://expressjs.com/en/4x/api.html#res.download) |          파일을 다운로드 하라는 메시지를 표시한다.           |
|  [res.end()](https://expressjs.com/en/4x/api.html#res.end)   |                  응답 프로세스를 종료한다.                   |
| [res.json()](https://expressjs.com/en/4x/api.html#res.json)  |                     JSON 응답을 보낸다.                      |
| [res.jsonp()](https://expressjs.com/en/4x/api.html#res.jsonp) |              JSONP지원으로 JSON 응답을 보낸다.               |
| [res.redirect()](https://expressjs.com/en/4x/api.html#res.redirect) |                     요청을 리디렉션한다.                     |
| [res.render()](https://expressjs.com/en/4x/api.html#res.render) |                   뷰 템플릿을 렌더링 한다.                   |
| [res.send()](https://expressjs.com/en/4x/api.html#res.send)  |                 다양한 타입의 응답을 보낸다.                 |
| [res.sendFile()](https://expressjs.com/en/4x/api.html#res.sendFile) |                파일을 옥텟 스트림으로 보낸다.                |
| [res.sendStatus()](https://expressjs.com/en/4x/api.html#res.sendStatus) | 응답 상태 코드를 설정하고 문자열 표시를 응답 본문으로 보낸다. |

<br>

### app.route()

app.route()를 사용해서 라우트 경로를 위한 연쇄 라우트 핸들러를 생성할 수 있다. 경로는 단일 위치로 지정되기 때문에, 중복성 방지 및 오타를 방지하기 위해서 등의 이유로 모듈화 된 라우트를 생성하는 것은 도움이 된다.

app.route()를 사용하여 연쇄 라우트 핸들러를 만드는 방법을 살펴보자.

```javascript
app.route('/book')
  .get(function (req, res) {
    res.send('Get a random book')
  })
  .post(function (req, res) {
    res.send('Add a book')
  })
  .put(function (req, res) {
    res.send('Update the book')
  })
```

<br>

###express.Router

express.Router을 사용하면 모듈화되고, 장착 가능한 라우트 핸들러를 생성할 수 있다. Router 인스턴스는 완전한 미들웨어이자 라우팅 시스템이다. 이러한 이유 때문에, 종종 "mini-app"이라고도 불린다.

다음 예시는 라우터를 모듈로 생성하고, 그 모듈 안에 미들웨어를 로드하고, 몇몇 라우트를 정의하고, 메인 앱의 경로에 라우터 모듈을 장착하는 것을 보여준다.

앱 디렉토리에 다음과 같은 내용을 가진 birds.js라는 이름의 라우터 파일을 생성한다.

```javascript
var express = require('express')
var router = express.Router()

// 이 라우터와 관련된 미들웨어
router.use(function timeLog (req, res, next) {
  console.log('Time: ', Date.now())
  next()
})
// 홈 페이지 라우터를 정의한다.
router.get('/', function (req, res) {
  res.send('Birds home page')
})
// about 라우트를 정의한다.
router.get('/about', function (req, res) {
  res.send('About birds')
})

module.exports = router
```

그런 다음 앱에 라우터 모듈을 로드한다.

```javascript
var birds = require('./birds')

// ...

app.use('/birds', birds)
```

이제 이 앱은 /birds 와 /birds/about 에 대한 요청을 다룰 수 있게 되었을 뿐만 아니라, 경로에  timeLog라는 특정한 미들웨어 함수를 호출 할 수 있게 되었다.