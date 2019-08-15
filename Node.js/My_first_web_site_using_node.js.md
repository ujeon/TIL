## Node로 첫번째 웹사이트를 만들어 보자!

<br>

###공식 문서 따라하기

```javascript
const http = require('http') // 노드에 존재하는 htttp 모듈을 가져온다.

const hostname = '127.0.0.1'; // 호스트 이름이다. 아마 ip주소인 것 같다.
const port = 3000; // 포트 번호를 뜻한다. 이따가 좀 더 알아보자.

// http 모듈의 createServer을 사용하면 문자 그대로 서버를 생성할 수 있다.
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

// listen 메서드는 연결을 수신하고 있는 HTTP 서버를 시작하는 메서드이다.
server.listen(port, hostname, () => {
  console.log('Server running at http://${hostname}:${port}/');
});
```

위의 코드를 실행하고 http://localhost:3000에 접속하면 'Hello World'가 표시되어 있을 것이다.

<br>

### 알고 갈 것(들)

#### 포트

문자그대로 배가 정박할 수 있는 항구와 의미가 유사하다.  컴퓨터나 프로그램은 인터넷에서 포트를 통해 '어딘가' 혹은 '어떤 것'에 연결된다. 포트 번호와 사용자의 IP 주소는 "누가 어떤 일을 하는지"에 대한 정보와 결합된다. 

