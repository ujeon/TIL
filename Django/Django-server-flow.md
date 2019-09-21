## 장고로 서버만들기

장고로 서버를 만들어서 JSON 응답을 내려주도록 해보자.

먼저 프로젝트 만드는 것부터 시작하자.

### 장고 프로젝트 시작하기

리액트-크리에이트-앱 처럼 장고도 프로젝트를 시작할 수 있는 명령어를 제공하고 있다. 물론 한줄한줄 코드를 타이핑하며 만들 수도 있겠지만, 나에겐 그럴 능력이 없다. 그러니 터미널을 켜고 원하는 디렉토리에 다음 명령어를 치자!

```
$ django-admin startproject project_name
```

프로젝트 이름은 원하는 프로젝트 이름을 치면 된다.

명령어를 입력하고 엔터를 누르면 다음과 같은 디렉터리, 파일을 자동으로 만들어 준다.

```
project_name/
    __pycache__/
    __init__.py
    settings.py
    urls.py
    wsgi.py
manage.py
```

각각의 파일이 어떤 역할을 하는지는 [MDN 문서를 참고](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/skeleton_website)하였다.

- \__init__.py 파일은 빈 파일이며, 파이썬이 현재 디렉토리를 파이썬 페키지로 다룰 수 있도록 지시한다.
- settings.py 파일은 모든 웹사이트 세팅을 포함하고 있다. 데이터 베이스 세부 설정, 정적인 파일 위치 그리고 우리가 만든 어플리케이션 등을 등록하는 곳이다.
- urls.py 파일은 사이트 url과 view를 매핑을 정의하는 파일이다. 모든 url 매핑 코드를 포함할 수 있지만, 특정한 어플리케이션에 일부를 할당해주는게 일반적이다.
- wigs.py 파일은 장고 어플리케이션이 웹 서버와 통신하는데에 사용된다. 이 파일을 보일러플레이트 처럼 다뤄도 된다.

manage.py 스크립트는 어플리케이션을 만들고, 데이터베이스와 함께 작업하고, 웹 서버를 개발하는데 사용되는 파일이다.

\__pycache__는 MDN 문서에 특별히 언급되어 있지 않다.

### 라우팅은 어디서 하지?

Express 프레임워크에서는 라우팅을 하기가 수월했다. 아니면 익숙해서 그런가?

장고에서의 라우팅은 나에게 있어서 조금 특별(?)하게 느껴졌다.

위의 파일 설명에서 알 수 있듯 urls.py 파일에서 url과 view를 매핑한다고 한다. 파일 이름부터 urls니 뭔가 냄새가 난다. 파일을 열어보자.

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls)
]
```

path라는 메서드 안에 'admin/'이라는 라우트가 존재한다!

그보다 먼저 path 메서드에 대해서 알아보자. [path 메서드는 다음과 같이 사용한다.](https://docs.djangoproject.com/en/2.2/ref/urls/#path)

>`path`(*route*, *view***,** *kwargs=None***,** *name=None*)

path의 첫 번째 파라미터는 route로, 이름 그대로 라우트를 인자로 받는다. 

두 번째 파라미터는 view인데, 해당 라우트에 대해 호출할 view 함수를 인자로 전달받는다.  혹은 [django.urls.include()](https://docs.djangoproject.com/en/2.2/ref/urls/#django.urls.include) 메서드를 인자로 받을 수 있다.

세 번째 인자 kwargs는 view 함수나 메서드에 추가적인 인자를 전달받는데 사용된다고 한다.

마지막 인자 name은 해당 url의 별명(?)을 짓는데 사용하는 것 같다.(확실하지 않아요.)

path가 위에서 설명한 인자들을 전달받으면 urlpatterns에 포함 될 엘리먼트를 반환한다.

특정 라우트로 요청이 들어오면 장고는 urlpatterns 리스트를 돌면서 첫번째로 일치하는 패턴에서 멈춘다. 일치하는 패턴을 찾으면 장고는 주어진 view를 가져와서 실행하게 된다. 또한 view를 호출할 때는 HttpRequest 인스턴스를 view에 전달한다.

위의 과정이 url을 view에 매핑하는 것이다.

#### *URL에서 특정 값 잡아내기*

URL에 연도, 월, 일과 같은 특정한 값이 포함된 경우가 있다. 경로에서 해당 값을 뽑아 내는 방법에 대해 알아보자. 먼저 다음과 같은 경로가 있다.

> /years/2019/i-will-be-changed

years 뒤에 연도를 나타내는 숫자는 2017, 2018, 2019 처럼 변할 수 있고, 뒤의 경로도 요청에 따라 변할 수 있다. 저러한 숫자, 문자를 잡아내기 위해서는 path 메서드에 전달하는 route에 꺾쇠를 사용하면 된다. 예시를 살펴보자.

```python
path('years/<int:year>/<slug:text>', views.year_archive)
```

view 함수는 다음과 같이 호출된다.

```python
views.year_archive(request, year=2019, text="i-will-be-changed")
```

물론 view 함수가 존재하는 파일에서도 해당 함수의 파라미터를 수정해야 한다.

[path를 확인하는데 사용되는 컨버터](https://docs.djangoproject.com/ko/2.2/topics/http/urls/)는 int, slug 이외에도 str, uuid 등이 있다.

___

### 뷰 작성하기

앞에서 라우팅하는 방법을 배웠으니 라우팅을 핸들링하는 [뷰 함수를 작성하는 방법](https://docs.djangoproject.com/ko/2.2/topics/http/views/)에 대해서 배워보자.

뷰에 대해 간략히 설명하면 뷰는 웹 요청을 받고 웹 응답을 하는 함수이다. 응답에는 웹 페이지 HTML 컨텐츠, 리디렉션, 404 에러와 같은 여러 에러, XML 문서, 이미지 등 여러가지가 있다. 

뷰가 무엇인지 간략히 살펴보았으니 사용하는 방법 등에 대해 알아보자.

다음은 현재 날짜와 시간을 반환하는 뷰 함수이다.

```python
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

위의 코드에 대해 설명하면, datetime  모듈을 사용해 현재 날짜, 시간 정보를 변수 now에 저장하고 해당 변수를 html에 삽입하여 응답으로 전달하는 뷰 함수이다.

 뷰는 생성된 응답이 포함된 HttpResponse 객체를 반환한다. (예외가 있기는 하다.)

### 에러 반환하기

응답 상태는 200, 201, 300, 404, 500 등 여러가지가 있다. 이러한 에러를 핸들링 하기위해서는 HttpResponse 메서드에 해당 상태를 전달해주면 된다.

```python
from django.http import HttpResponse

def my_view(request):
    # ...

    # 201 상태 코드를 전달한다.
    return HttpResponse(status=201)
```

### 뷰 데코레이터

[django.views.decorators.http](https://docs.djangoproject.com/ko/2.2/topics/http/decorators/#module-django.views.decorators.http) 안에 있는 데코레이터를 사용하면 요청 메서드에 따라 뷰의 접근을 제한 할 수 있다.

데코레이터는 다음과 같이 생겼다.

>`require_http_methods`(*request_method_list*)

사용방법은 다음과 같다.

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"]) # 아래 my_view는 GET, POST 요청에만 응답한다.
def my_view(request):
    pass
```

데코레이터가 필요한 뷰 함수 위에 데코레이터를 붙여주면 된다. 

___

### 요청과 응답 객체

장고는 [요청 및 응답 객체](https://docs.djangoproject.com/en/2.2/ref/request-response/#django.http.HttpResponse)를 사용하여 시스템을 통해 상태를 전달한다.

클라이언트에서 요청이 오면 장고는 요청에 대한 메타 데이터가 담긴 HttpRequest 객체를 생성한다. 그런 다음 장고는 적절한 뷰를 로드한 다음 HttpRequest를 첫 번째 인자로 뷰 함수에 전달한다. 각각의 뷰 함수는 HttpResponse 객체를 반환할 필요가 있다.

[요청과 응답 객체의 속성에 관한 것은 해당 문서를 참고하자.](https://docs.djangoproject.com/en/2.2/ref/request-response/#django.http.HttpResponse)

###JsonResponse 객체를 사용하여 JSON으로 응답하기

장고에는 응답으로 전달 할 내용을 JSON으로 변경해주는 객체가 있다. JsonResponse 클래스는 다음과 같이 생겼다.

>*class* `JsonResponse`(*data*, *encoder=DjangoJSONEncoder*, *safe=True*, *json_dumps_params=None, **kwargs*)

JsonResponse 객체의 기본 Content-Type 헤더는 application/json으로 설정되어 있다.

첫 번째 파라미터인 data는 dict 인스턴스여야 한다. 만일 safe 파라미터가 False로 설정되어 있다면, 어느 JSON-serializable 객체일 가능성이 있다.

encoder 파라미터의 기본 값으로 설정된 [django.core.serializers.json.DjangoJSONEncoder](https://docs.djangoproject.com/en/2.2/topics/serialization/#django.core.serializers.json.DjangoJSONEncoder)는  데이터를 직렬화하는데 사용된다.

safe 불리언 파라미터는 기본적으로 True로 설정되어 있다. 만일 safe가 False로 설정된다면, 어느 객체든 직렬화가 가능하다.(그렇지 않으면 dict 인스턴스만 허용된다.) 만일 safe가 True이고 dict 객체가 아닌 객체가 data에 전달된다면 TypeError가 발생한다.

json_dumps_params 파라미터는 응답을 생성할 때 사용되는 json.dumps()를 호출할 때 전달하는 키워드 인자들의 딕셔너리다.

JsonResponse에 대해 살펴보았으니 사용방법에 대해 알아보자.

```python
from django.http import JsonResponse
response = JsonResponse({'foo': 'bar'})
response.content # b'{"foo": "bar"}'
```

딕셔너리가 아닌 객체를 직렬화 하려면 다음과 같이 작성하면 된다.

```python
response = JsonResponse([1, 2, 3], safe=False)
```

