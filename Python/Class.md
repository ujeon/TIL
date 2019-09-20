## 클래스

클래스는 데이터 형식의 템플릿이다.

클래스를 만들고 클래스의 바디를 비워놓으면 에러가 발생한다. (클래스가 아니라 함수도 마찬가지인 것 같다.) 따라서 의도적으로 바디를 비워놓았음을 의미하는 pass를 표시해야한다.

```python
class MyFirstClass: # 바디가 비어있다는 에러가 발생한다.
  
class MyFirstClass: # 따라서 pass를 전달해주어야 한다.
	pass
```

파이썬 클래스는 상속하여 사용한다.

```python
my_instance = MyFirstClass()
```

type() 메서드를 사용하여 클래스 인스턴스의 데이터 형태를 확인해보면

```python
print(type(my_instance))
# prints "<class '__main__.MyFirstClass'>"
```

\__main__은 "현재 실행하고 있는  파일"이라는 의미를 가지고 있다. 즉, MyFirstClass은 현재 스크립트에 정의되어 있다는 의미이다.

### 클래스의 변수

```python
class Music:
  genre = "Electronic"
    
my_music = Music()
print(my_music.genre) # "Electronic"가 표시된다.
```

클래스에 변수를 선언하면 인스턴스에서도 해당 변수에 접근이 가능하다. 또다른 인스턴스에서도 역시 접근이 가능하다.

### 메서드

메서드는 클래스의 일부로 정의된 함수다. 항상 메서드의 첫번째 인자는 해당 메서드를 호출하는 객체가 전달된다. 일반적으로 self라는 단어를 사용한다.

```python
class Music:
  genre = "Electronic"
  
  def describe_genre(self):
    print("{} music is kind of upbeat musics".format(self.genre))
    
my_music = Music()
my_music.describe_genre() # "Electronic music is kind of upbeat musics"
```

물론 self 이외에도 다른 인자를 전달받을 수 있다.

### Constructors

파이썬 클래스에는 특별한 메서드들이 존재한다. 이 메서드들은 "magic" 혹은 dunder 메서드라고 불리는데, dunder 메서드라고 불리는 이유는 메서드에 언더스코어가 앞뒤로 두개씩 붙기 때문이다.

이 dunder 메서드들 중 하나인 \__init__ 메서드는 생성자 역할을 한다. 따라서 클래스가 인스턴스 될 때마다 호출된다.

```python
class Dog:
  def __init__(self):
    print("bark! bark!")
    
my_dog = Dog() # 'bark! bark!' 나도 짖고
your_dog = Dog() # 'bark! bark!' 너도 짖고
```

가만 보면 Dog()에 인자를 전달 해줄 수 있을 것만 같다.

그렇다. 전달 해줄 수 있다.

Dog()에 전달된 인자는 \__init__이 받게 된다.

```python
class Dog:
  def __init__(self, emotion):
    if emotion == "angry":
      print("grrrrrrrrrrr...")
    elif emotion == "afraid":
      print("wr....")
    else:
    	print("bark! bark!")
    
my_dog = Dog("angry") # 'bark! bark!' 나도 짖고
your_dog = Dog("afraid") # 'bark! bark!' 너도 짖고
```

Dog()을 호출 할 때 인자를 전달 함으로써 서로 다른 결과를 표시하고 있음을 알 수 있다.

### 인스턴스 변수

서로 같은 클래스에서 파생된 두 인스턴스는 같은 이름을 가진 변수를 가질 수 있다. 무슨 의미냐면,

```python
class Animal:
  pass

animal_1 = Animal()
animal_2 = Animal()

animal_1.kind = "Birds"
animal_2.kind = "Cats"

print(f"{animal_1.kind}, {animal_2.kind}")	# Birds, Cats
```

이름이 같더라도 값이 재설정 되지 않고 서로 다른 값을 가지고 있는 것을 확인할 수 있다. 즉, 인스턴스 객체는 자신만의 고유한 변수를 가질 수 있다는 것이다.

### 속성 함수

클래스나 인스턴스의 변수 아닌 속성에 접근하려고 하면 파이썬은 AttributeError를 반환하게 된다.

```python
class NoCustomAttributes:
  pass

attributeless = NoCustomAttributes()

try:
  attributeless.fake_attribute
except AttributeError:
  print("The attribute isn't belong to class nor instance!")

# prints "The attribute isn't belong to class nor instance!"
```

그렇다면 클래스나 인스턴스가 어떠한 속성을 가지고 있는지 어떻게 판별할 수 있을까?!

바로 hasattr() 혹은 getattr() 메서드를 사용하면 된다.

```python
hasattr(attributeless, "fake_attribute")
# False를 반환한다.

getattr(attributeless, "other_fake_attribute", 800)
# 기본 값인 800을 반환한다.
```

### Self

파이썬에는 키-값을 저장할 수 있는 딕셔너리가 이미 존재하므로 해당 목적을 위해서 객체를 사용하는 것은 유용하지는 않다. 객체가 담고있는 정보에 엄격성을 부여하면 인스턴스의 변수는 강력해진다.

이는 생성자가 전달받은 인자를 사용하여 인스턴스 변수를 생성할 때 명백하게 나타난다.

만일 검색 엔진을 만들고 있고, 개별 항목에 대한 클래스를 만들고 싶다면 다음과 같이 할 것이다.

```python
class SearchEngineEntry:
  def __init__(self, url):
    self.url = url

mdn = SearchEngineEntry("www.mdn.com")
w3schools = SearchEngineEntry("www.w3schools.com")

print(mdn.url)
# "www.mdn.com"

print(w3schools.url)
# "www.w3schools.com"
```

self 키워드 호출되는 클래스가 아닌 객체를 참조하므로 SearchEngineEntry에서 엔트리에 보안 링크를 반환하는 secure 메서드를 정의할 수 있다.

```python
class SearchEngineEntry:
  secure_prefix = "https://"
  def __init__(self, url):
    self.url = url

  def secure(self):
    return "{prefix}{site}".format(prefix=self.secure_prefix, site=self.url)

mdn = SearchEngineEntry("www.mdn.com")
w3schools = SearchEngineEntry("www.w3schools.com")

print(mdn.url)
# "https://www.mdn.com"

print(w3schools.url)
# "https://www.w3schools.com"
```

SearchEngineEntry에 self를 인자로 갖는 secure 메서드를 정의하여 URL에 보안 prefix를 추가할 수 있도록 만들었다. 이것이 객체 지향 프로그램의 장점이다.

### 모든 것은 객체야

인스턴스화 된 후 사용자 정의 객체에 속성을 추가할 수 있으므로, 객체의 생성자에 명시적으 정의되지 않은 일부 속성이 객체에 존재할 수 있다.(무슨말이야?)

dir()메서드를 사용하여 런타임에서 객체의 속성을 조사할 수 있다. dir은 디렉토리의 약자이며 객체의 속성을 잘 정돈하여 보여준다.

```python
class FakeDict:
  pass

fake_dict = FakeDict()
fake_dict.attribute = "Cool"

dir(fake_dict)
# Prints ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'attribute']
```

엄청 많다. 그리고 모든 속성이 double-underscore로 표시되어 있다.

파이썬의 기본 데이터 유형에 type()을 사용할 수 있는 것도 바로 그것들이 객체이기 때문이다.(int, float, str, list 그리고 dict)

이들 파이썬 클래스들은 자신들의 인스턴스를 위해서 특별한 문법을 가지고 있다.

```python
temp_list = [10, "string", {'abc': True}]

type(temp_list)
# Prints <class 'list'>

dir(temp_list)
# Prints ['__add__', '__class__', [...], 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

위의 예시를 통해서 temp_list가 list 클래스의 인스턴스임을 알 수 있고, dir()메서드를 사용해서 dunder 속성들과 일반적인 list 메서드 목록을 확인할 수 있다.

### 문자열로 표현하기

디버깅을 하기 위해서 필요한 정보를 출력하는 것은 중요한 일인데, 파이썬에서 객체를 출력해보면 영 좋지 않은 정보를 볼 수 있을 것이다.

```python
class Musician():
  def __init__(self, name):
    self.name = name

queen = Musician("Queen")
print(queen)
# prints "<__main__.Musician object at 0x105d7c990>"
```

queen 인스턴스를 print() 메서드로 출력해보면 요상한 문자열에 Musician 객체가 존재한다고 알려준다. 0x105d7c990은 메모리 주소를 의미하며 해당 주소에 객체가 존재한다는 의미가 될 것이다.

메모리 주소를 알려주는 것도 좋겠지만, 디버깅 하는데는 별 소용이 없다. 아무래도 알아보기 쉽게 바꾸는 것이 디버깅하기에는 좋을 것 같다.

앞서 dunder 메서드의 의미와 dunder 메서드 중 하나인 \__init__에 대해서 살펴보았다.

이제 또 다른 메서드 \__repr__ 에 대해서 알아보자. 이 메서드를 사용하면 파이썬에게 클래스를 문자열로 표현해달라고 요청할 수 있다. 무슨 말인지 잘 모르겠으니 일단 사용해보고 위의 코드와 비교해보자.

\__repr__는 오직 self만을 파라미터로 가지며 문자열을 반환한다.

```python
class Musician():
  def __init__(self, name):
    self.name = name
    
	def __repr__(self):
    return self.name
  
queen = Musician("Queen")
print(queen)
# prints "Queen"
```

앞서 살펴본 코드와는 다르게 print()메서드가 Queen이라는 문자열을 반환하고 있는 것을 확인할 수 있다.  