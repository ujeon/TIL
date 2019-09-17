###robots.txt가 뭘까?!

robots.txt는 웹사이트에서 웹 크롤링과 같은 프로그램의 접근을 제어위한 규약이다. robots.txt는 웹사이트 최상위 경로에 존재하기 때문에 사이트 URL을 입력하고 robots.txt를 입력하면 확인할 수 있다.

> https://www.example.com/robots.txt

예를 들어 구글에 robots.txt를 입력하게 되면 다음과 같은 결과를 받아볼 수 있다.

```
User-agent: *
Disallow: /search
Allow: /search/about
Allow: /search/static
Allow: /search/howsearchworks
Disallow: /sdch
Disallow: /groups
Disallow: /index.html?
Disallow: /?
Allow: /?hl=
Disallow: /?hl=*&
Allow: /?hl=*&gws_rd=ssl$
Disallow: /?hl=*&*&gws_rd=ssl
Allow: /?gws_rd=ssl$
Allow: /?pt1=true$
Disallow: /imgres
Disallow: /u/
Disallow: /preferences
Disallow: /setprefs
Disallow: /default
Disallow: /m?
Disallow: /m/
Allow:    /m/finance
......etc
```

<br>

###어떻게 사용할까?

특정 디렉토리의 접근을 허용하려면

```
User-agent: * //User-agent는 사용자를 대신하여 일을 수행하는 소프트웨어 에이전트라고 한다.(*은 모든 User-agent인 듯?!)
Allow: /search/about
```

특정 디렉토리의 접근을 차단하려면

```
User-agent: * 
Disallow: /search
```

모든 디렉토리의 접근을 허용하려면

```
User-agent: *
Allow: /
```

모든 디렉토리의 접근을 차단하려면

```
User-agent: * 
Disallow: /
```

<br>

### 왜 사용할까?

앞서 말한 것처럼 웹 사이트에 크롤링등을 하는 로봇이 접근하는 것을 막기 위함을 목적으로 한다. 로봇이 해당 robots.txt를 읽고 접근을 중지하도록 한다. 그렇기 때문에 접근 방지를 설정하였어도 다른 사람들이 그 파일에 접근할 수는 있다. 