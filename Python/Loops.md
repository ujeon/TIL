## 루프

파이썬 반복문을 알아보자

### for문 만들기

일반적으로 파이썬에서 for문은 다음과 같이 사용한다.

```python
for <temporary variable> in <list variable>:
    <action>
```

예를 들어

```python
dog_breeds = ['french_bulldog', 'dalmation', 'shihtzu', 'poodle', 'collie']
for breed in dog_breeds:
    print(breed) 
# french_bulldog 
# dalmation
# shihtzu
# poodle
# collie
```

위의 예제는 리스트 dog_breeds에서 각 요소를 반복하여 하나씩 출력하게 된다. 반복문 안의 코드가 들여쓰기가 되어있지 않다면 IndentationError가 발생한다는 것을 명심하자.

### 루프에 범위 사용하기

어떤 내용을 n번 반복하고 싶다. 그렇다면 반복문에 사용할 리스트가 꼭 필요할까?? 그렇지는 않다. 무언가를 단순히 n번 반복하고 싶다면 어떤 정보가 담긴 리스트를 꼭 만들 필요가 없다.

대신 range() 메서드를 사용하면된다. range() 매서드에 숫자를 전달하면 해당 숫자만큼의 길이를 가진 리스트를 반환해준다.

```python
zero_thru_five = range(6)
# zero_thru_five = [0, 1, 2, 3, 4, 5]
```

위의 예처럼 말이다. range()의 역할을 알아보았으니, 이제 어떤 동작을 반복할때 사용하기만 하면 된다!

```python
for i in range(6)
	print("Hello World!")
```

### 무한 루프

다음의 코드를 실행하면 어떻게 될까?!

```python
test_numbers = [1, 2, 3]

for num in test_numbers:
  test_numbers.append(4)
```

아마 test_numbers에 4가 계속해서 삽입되어 리스트가 무한정 늘어날 것이다.

루프를 이렇게 사용하지 말자.

### break

반복문의 목적을 달성하면 중간에 멈추고 싶을때가 있다. 예를 들어 리스트가 100,000개 이상인 경우에 어떤 값을 일찍 찾았는데 그 이후 요소까지 검사하면 엄청난 낭비다!

이러한 경우 반복문을 멈출 필요가 있는데, 이 때 break를 사용한다.

break는 다음과 같이 사용한다.

```python
items_on_sale = ["blue_shirt", "striped_socks", "knit_dress", "red_headband", "dinosaur_onesie"]

print("Checking the sale list!")
for item in items_on_sale:
  print(item)
  if item == "knit_dress":
    break
print("End of search!")

# Checking the sale list!
# blue_shirt
# striped_socks
# knit_dress
# End of search!
```

 break를 사용하였기 때문에 "knit_dress"이후의 요소는 살펴보지 않게 된다.

### continue

반복문을 break로 멈췄다면, 다시 재개 시킬 수 도 있다. continue 키워드를 사용하면 다음 요소로 넘어가게 된다. 

말로 설명하면 잘 모르겠으니 코드를 살펴보자

```python
big_number_list = [1, 2, -1, 4, -5, 5, 2, -9]

for i in big_number_list:
  if i < 0:
    continue
  print(i)
  
  # 1
  # 2
  # 4
  # 5
  # 2
```

i가 0보다 작다면 i를 출력하지 않고 다음 요소로 넘어가게 된다.

###while 루프

for 루프 이외에도 while 루프를 사용하여 반복적인 작업을 할 수 있다.

while문은 다음과 같이 사용한다.

```python
dog_breeds = ['bulldog', 'dalmation', 'shihtzu', 'poodle', 'collie']

index = 0
while index < len(dog_breeds):
  print(dog_breeds[index])
  index += 1
```

while문은 조건이 거짓이 될 때까지 안의 코드를 반복한다. 

while문은 몇 번이나 반복해야 할 지 모를 때 유용하다.

### 중첩 루프

리스트 안에 리스트가 들어있는 경우는 어떻게 개별 요소에 접근할 수 있을까?

간단히 반복문을 두 번 사용하면 된다! (안될 땐 반복문!)

```python
project_teams = [["Ava", "Samantha", "James"], ["Lucille", "Zed"], ["Edgar", "Gabriel"]]

for team in project_teams:
  for student in team:
    print(student)
    
# Ava
# Samantha
# James
# Lucille
# Zed
# Edgar
# Gabriel
```

### List Comprehensions

파이썬은 반복문을 조금 특별한 방식으로 사용할 수 있다.

다음과 같은 정보가 담긴 리스트가 있다고 가정하자.

```python
words = ["@bird8920", "#nostelgia", "@choi152", "*sad*", "#hot dog", "@cinnamon12", "friends", "#hot place"]
```

words 리스트에서 맨 처음 문자가 "@"인 단어를 추출하려고 한다. "@"로 시작하는 단어는 유저 이름이다. 이 유저 정보를 담기 위해서 먼저 빈 리스트를 만들고 usernames라 이름 짓자.

어떻게 하면 "@"로 시작하는 단어를 골라낼 수 있을까? 바로 반복문을 사용하는 것이다.

```python
words = ["@bird8920", "#nostelgia", "@choi152", "*sad*", "#hot dog", "@cinnamon12", "friends", "#hot place"]
usernames = []

for word in words:
  if word[0] == '@':
    usernames.append(word)
```

위의 코드를 실핼하게 되면, "@"로 시작하는 단어가 usernames에 담기게 된다.

```python
>>> print(usernames)
["@bird8920", "@choi152", "@cinnamon12"]
```

파이썬은 위의 과정을 좀더 간결하게 작성할 수 있는 방법을 제공한다.

바로 리스트 안에 반복문을 직접 사용하는 것이다!

```python
usernames = [word for word in words if word[0] == '@']
```

간결해 보이기는 하지만 익숙해지기 까지는 시간이 좀 필요할 것 같이 생겼다.

어쨌든 이 방법을 list Comprehensions라고 한다.

### List Comprehensions에 대해 조금 더 알아보자

이전 예제를 가지고 list comprehensions를 응용해보자

```python
>>> print(usernames)
["@bird8920", "@choi152", "@cinnamon12"]
```

각 요소마다 요소 앞에 "Welcome! "이라는 말을 붙이고 이를 messages라는 리스트에 담을 것이다.

list comprehensions을 사용해보면

```python
messages = ["Welcome! " + user for user in usernames]
```

messages리스트를 살펴보면 다음과 같을 것이다.

```python
["Welcome! @bird8920", "Welcome! @choi152", "Welcome! @cinnamon12"]
```

코드를 여러 줄 사용하는 것 보다는 간결하기는 하다!

