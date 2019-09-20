## 제어

파이썬 제어문에 대해서 배워보자

### Boolean 표현

파이썬에도 True, False 표현이 존재한다. 어떤 조건이 참이거나 거짓인 경우에 사용된다.

### 관계 연산자: 같거나, 같지 않거나

관계연산자는 두 대상을 비교해서 참인 경우 True를 거짓인 경우 False를 반환한다. 같은지 여부를 비교할 때에는 ==을 사용하며, 다른지 여부를 비교할 때에는 !=을 사용한다. 

```python
>>> 1 == 1
True
>>> 10 == 1
False
>>> 10 != 1
True
>>> '1' == 1
False
```

마지막은 왜 False를 반환할까? 바로 '1'은 문자열 데이터이고, 1은 숫자 데이터이기 때문이다. 즉, 데이터형태가 같은지 여부도 확인한다.

### Boolean 변수

True와 False는 bool이라는 타입을 가진다. 또한 이 bool 타입을 변수에 할당할 수도 있다.

### if 구문

파이썬에서는 if 구문을 다음과 같이 사용한다.

```python
if is_it_true:
	yes_it_is()
```

간단하다.

### 두번째 관계 연산자

두 번째로 알아볼 관계 연산자는

- 비교 대상보다 크다면: \>
- 비교 대상보다 작다면: <
- 비교 대상보다 크거나 같다면: \>=
- 비교 대상보다 작거나 같다면: \<=

### Boolean 연산자: and

조건 구문에서 한번에 두개 이상의 조건을 포함시키고 싶을 때가 있다. 그럴때는 boolean 연산자를 사용하면 된다. 파이썬에는 다음과 같이 세 개의 boolean 연산자가 있다.

- and
- or
- not

이 중 and 연산자는 두 개의 조건이 각각 참인지 평가한 후에 둘 다 참인 경우에는 True를, 둘 중 하나라도 거짓인 경우에는 False를 반환한다.

and 연산자 사용방법을 살펴보자

```python
>>> (1 + 1 == 2) and (2 + 2 == 4)
True
>>> (1 + 1 == 2) and (2 < 1)
False
>>> (1 > 9) and (5 != 6)
False
>>> (0 == 10) and (1 + 1 == 1) 
False
```

두 번째, 세 번째 예시를 보면, 둘 중에 하나가 참이더라도 다른 하나가 거짓이면 False를 반환함을 알 수 있다.

### Boolean 연산자: or

or 연산자는 두 조건문 중 하나가 참이거나 둘다 참인 경우에는 True, 두 조건문 모두 거짓인 경우 False를 반환한다.

```python
>>> True or (3 + 4 == 7)
True
>>> (1 - 1 == 0) or False
True
>>> (2 < 0) or True
True
>>> (3 == 8) or (3 > 4) 
False
```

마지막 예시에서 두 조건문 모두 거짓이면 False를 반환하는 것을 알 수 있다.

### Boolean 연산자: not

마지막 boolean 연산자 not은 boolean 표현식에 사용되어 해당 boolean 값을 뒤집는 역할을 한다.

not 연산자 사용방법을 살펴보자

```python
>>> not 1 + 1 == 2 # 1 + 1 == 2 는 True인데, 앞에 not을 붙임으로써 False가 된다.
False
>>> not 7 < 0
True
```

### else 구문

else 문은 언제나 if 문과 함께 쓰인다. if 문에서 조건이 거짓인 경우 else 문으로 이동하게 된다.

```python
def check_age(age):
  if age > 19:
    return "You are enough to drink."
  else:
    return "Do not hold the cup."
```

### else if 구문

파이썬에서 elif 구문은 if 구문 다음에 elif 를 사용하면 if 구문이 거짓인 경우 elif 구문으로 이동하여 새로운 조건으로 상태를 제어할 수 있다.

```python
def size_recommend(height):
  if height <= 160:
    return "This size is the best for you!"
  elif (height > 160) and (height <= 170):
    return "Well, This one is good for you"
  else:
    return "Oh, You are so tall. Follow me, There are things for you"
```

### Try 그리고 Except 구문

if 구문 이외에도 프로그램을 제어 할 수 있는 방법이 있다. 바로 try, except 구문을 사용하는 것이다. try, except 구문을 사용해서 사용자가 마주칠 수 있는 에러를 확인할 수 있다.

일반적으로 try, except 구문은 다음과 같이 사용한다.

```python
try:
    # some statement
except ErrorName:
    # some statement
```

  먼저 try 구문안에 있는 코드가 실행된다. 이 코드들이 실행되는 동안 NameError 혹은 ValueError와 같은 예외 상황이 발생하고 해당 예외 상황이 except 문의 키워드와 일치하면 try 문이 종료되고 except문이 실행된다.

다음 예시를 살펴보자

```python
def divides(a,b):
  try:
    result = a / b
    print (result)
  except ZeroDivisionError:
    print ("Can't divide by zero!")
```

다음 코드는 a 를 b 로 나누어 결과를 출력하는 코드다. 만일 b에 0이 입력되면 a를 0으로 나누는 상황이 발생한다. 어떤 숫자를 0으로 나누게 되면 파이썬은 ZeroDivisionError을 반환하게 된다. 즉, 위에서 말한 예외 상황이 발생하는 것이다. 따라서 try 구문이 종료되고 except에 있는 print() 메서드를 실행하게 된다.