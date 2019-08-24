### 01 - JavaScript Scopes

```javascript
var x = 10;
function outer () {
  var x = 20;
  function inner () {
    x = x + 10;
    return x;
  }
  inner();
}

outer();

var result = x;
```

outer를 실행하면 스코프 안에 새로운 변수 x가 선언되고 값 20이 담긴다. 이어서 Inner 함수가 정의되고 실행되는데...  x = x + 10이라 뭔가 20 + 10 = 30이 될 것 같지만 outer 함수가 inner()를 리턴하고 있지 않으므로 결과는 10. 따라서 정답은 10이 된다.



```  javascript
var x = 10;
function outer () {
  x = 20;
  function inner () {
    var x = x + 20;
    return x;
  }
  inner();
}

outer();

var result = x;
```

var x = 10이 선언되어 있고, 결과에는 x를 담고 있다. outer 함수를 실행하면  x = 20으로 x에는 20이 담기게 된다. 이어서 inner를 실행하게 되면 새로운 변수 x가 선언되고 x + 20이 담기게 되는데, 새로운 변수 x는 전역에 선언된 x를 참조하여 40이 될 것 같지만... 실은 전역에 선언된 변수 x가 아니다. 바로 새로 선언된 x이다. x는 선언만되고 할당은 아직 안되었기 때문에(왼쪽에서 부터 실행된다고 보면) x + 20은 undefined가 되고, 결과적으로 inner가 반환하는 값은 NaN이 된다.

결과적으로 outer에서 실행한 x = 20이 함수 밖에 있는 x에 영향을 주어 result에는 20이 담기게 된다. 따라서 정답은 20.

<br>

### 02 - Closures

무사 통과!!

클로저는 함수가 실행이 끝나더라도 해당 함수 안에 정의된 변수에는 여전히 접근이 가능한 상태를 의미한다. 그리고 아무리 함수를 리턴한다고 하여도 바깥 함수의 변수를 사용하지 않는다면 클로저라고 볼 수 없다.

<br>

### 03 - Keyword 'this'

무사 통과!!!

this 가리키는 문맥은 this가 어디서 불려지는지(?)에 달려 있다. 만약 this가 전역이나 일반적인(바인딩 되거나 특정 객체의 메서드가 아니라면) this가 가리키는 것은 window 객체가 된다. 하지만 특정 객체 안의 메서드로써 실행된다면 this는 그 객체를 가리키게 된다. 생성자 함수에서 new 키워드를 통해 새로운 인스턴스가 생성될 때 this는 그 인스턴스를 가리키게 되고, 만약 함수를 특정 객체로 묶어주고 싶어서 bind, apply, call 등을 사용하게 되면 this는 그 묶어준 객체가 된다.

<br>

### 04 - JS Prototypes

....프로토 타입.......

일단 시작하기에 앞서서 Object.create를 살펴보자. mdn에서.

>Object.create(proto[,propertiesObject])
>
>매개변수
>
>proto
>
>> ​	새로 만든 객체의 프로토타입이어야 할 객체
>
>propertiesObject
>
>> ​	선택사항이다. 지정되고 undefined가 아니면, 자신의 속성이 열거가능한 객체는 해당 속성명으로 새로 만든 객체에 추가될 속성 설명자를 지정한다.
>
>반환값
>
>지정된 프로토타입 개체와 속성을 갖는 새로운 개체

라고 한다.

반환값만 보고 이야기하면 Object.create(obj1)의 반환값을 변수에 저장하게 되면 그 변수는 객체가 되고, obj1가 그 변수 객체의 프로토타입이 된다.

```javascript
var obj1 = { x: 10 };
var obj2 = Object.create(obj1);

var result = obj2.x;

// 자세히 살펴보자!
console.log(obj2); // {}
obj2.x = 100;
console.log(obj2); // { x: 100 }
console.log(obj2.__proto__.x); // 10
```

Object.create(obj1)의 결과를 obj2에 할당하고 있다. 따라서 obj2의 프로토타입은 obj1이 되며 obj2.`__`proto`__`.x를 살펴보면(혹은 obj.x) 10이 담겨있는 것을 알 수 있다. 그렇다면 obj1의 x 프로퍼티가 obj2의 프로퍼티가 된 것일까?! 그렇지는 않다! obj2.x를 콘솔에서 살펴보면 10이 나오긴 하지만 그 값이 저장되어 있는 곳은 `__`proto`__`이다. 마치 x가 obj2의 프로퍼티 인 것처럼 눈속임(?)을 한 것이라 할 수 있다. x가 obj2의 프로퍼티가 아니기 때문에,  obj2.x에 100을 할당할 수 있고, obj2.x === obj.`__`proto`__`.x 는 false가 된다. 따라서 result에는 10이 담기게 된다.



```javascript
var obj1 = { x: 10 };
var obj2 = Object.create(obj1);
var obj3 = Object.create(obj1);

obj1.x = 15;

var result = obj2.x + obj3.x;
```

obj2와 obj3 각각에 obj1을 obj2, obj3의 프로토타입으로써 할당해주고 있다. 그리고 obj1.x의 값을 10에서 15로 변경해주고 있다. obj2와 obj3의 프로토타입은 값을 복사 받은 것이 아니라 obj1의 객체를 참조하고 있기 때문에 obj2 + obj3 의 결과는 20이 아니라 30이 된다.



```javascript
var obj1 = { x: 10 };
var obj2 = Object.create(obj1);

obj2.x += 10;
obj1.x = 15;

var result = obj2.x;
```

이거.. 완전 헷갈린다. obj2.x = obj2.x + 10; 은 마치 obj2 프로토타입의 x의 값을 20으로 변경시킬 것 같지만, 사실은 obj2에 x라는 프로퍼티를 설정하고 그 값을 20으로 설정하게 된다. 이어서 obj1.x의 값을 15로 변경하고 있는데, 이는 obj2 프로토타입의 x에도 영향을 미친다. 콘솔에 obj2를 찍어보면 x의 값은 20으로, 프로토타입의 x는 15임을 알 수 있다. result에는 obj2의 x 프로퍼티가 담겨 있으므로 정답은 20.



```javascript
var obj1 = { x: 10 };
var obj2 = Object.create(obj1);
var obj3 = Object.create(obj2);

var result = obj3.x + 10;
```

프로토타입 체인 문제인 것 같다. obj2의 proto는 obj1이 되고, obj3의 proto는 obj2가 된다. 따라서 obj3.`__`proto`__`.`__`proto`__`.x 는 10이 되므로 결과는 20이 된다.



```javascript
var obj1 = { x: 10 };
var obj2 = Object.create(obj1);
var obj3 = Object.create(obj2);

obj2.x = 20;

var result = obj3.x + 10;
```

 헷갈린다. obj2의 proto는 obj1이 되고, obj3의 proto는 obj2가 되는 형식이다. 중간에 obj2.x = 20; 을 실행하고, 이 결과로 obj2에는 값이 20인 x 프로퍼티가 생기게 된다. obj3는 이 obj2를 자신의 proto로 가지기 때문에,  obj3.`__`proto`__`.x는 20, obj3.`__`proto`__`.`__`proto`__`.x 는 10이 된다. obj3.x 는 20을 참조하기 때문에 정답은 30이 된다.

<br>

### 05 - Big O

What is the Big O time complexity for removing an element at a specific index from an array?

배열에서 특정 인덱스를 알고 그 값을 지운다고 하더라도 시간 복잡도는 O(n)이 된다. 왜냐하면 지우고 난 빈자리를 뒤의 요소들이 이동하면서 채워야하기 때문이다.

<br>

### 06 - Keyword 'new'

new 키워드는 pseudoclassical 한 상속 패턴에 사용되는 키워드이다.  새로운 인스턴스를 만들 수 있는 생성자 함수를 정의하고 new 키워드를 사용하여 실행하는 방식으로 상속을 구현한다.

<br>

### 07 - Function Binding

```javascript
var name = "Window";
var alice = {
  name: "Alice",
  sayHi: function() {
    alert(this.name + " says hi");
  }
};
var bob = { name: "Bob" };
setTimeout(function() {
  alice.sayHi();
}, 1000);
```

setTimeout 함수는 스택에서 동기적으로 실행되는 것이 아니라 웹 API 영역에서 실행된다. 그렇기 때문에 위의 문제에서 setTimeout은 1초동안 콜백함수를 가지고 있다가 콜백함수를 큐로 전달한다. 그러고 난 후 이벤 루프가 콜스택을 살펴보다가 콜스택이 비어있으면 큐에 대기하고 있던 콜백 함수를 쌓아올린다. 콜백함수가 실행되면 안에 있는 alice.sayHi() 를 실행하게 되는데, alice 객체의 메서드로서 호출하고 있으므로 this는 alice 객체가 된다.

<br>

### 08 - JavaScript Callbacks

 함수는 콜백 함수를 전달받아서 실행할 수 있다. 비동기 처리도 콜백함수를 이용해서 할 수 있기도 하다.

<br>

### 09 - Value vs. Reference

```javascript
var player = { score: 3 };
function doStuff(obj) {
  obj.score = 2;
  obj = undefined;
}
doStuff(player);
```

객체는 주소를 참고한다. obj.score = 2는 player의 score를 2로 변경하게 된다. obj = undefined는 아무런 영향을 끼치지 않는다. 따라서 player.score은 결론적으로 2가 된다.



```javascript
var player = { score: 3 };
function doStuff(obj) {
  obj = {};
}

player = doStuff(player);
```

doStuff는 아무것도 반환하지 않으니 undefined가 된다. 참고로 obj = {};를  실행하여도 전역 변수 player = { player: 3 }에는 영향이 없다. 물론 앞서 말한 이유로 undefined로 바뀌긴 하겠지만?

<br>

### 10 - Order of Execution

```java
var x = 10;
function f () {
  x = x + 1;
}

var result = x;

f();
```

result를 f 함수 실행하기 전에 확인해보면 10일 것이다. 하지만 실행이 끝나고 확인해보면 11일 것이다.



```javascript
var x = 10;
var obj = {
  y: x,
  z: obj.y + 1
};

var result = obj.z;
```

obj.z 값이 11이 될 것 같지만 사실은 'y' of undefined 에러가 뜬다. obj가 선언은 되지만 아직 할당은 되지 않은 상태이기 때문이다.



배열이나 객체의 값(?) 역시 왼쪽에서 오른쪽으로 실행된다고 생각하자!

<br>

### 11 - Recast.ly

리액트 라이프 사이클 API를 공부하도록 하자

<br>

### 12 - Chatterbox Server

```javascript
console.log("A");
setTimeout(function() {
  console.log("B");
}, 500);

setTimeout(console.log("C"), 1000);
```

마지막 setTimeout의 콜백함수는 즉시 실행된다. 따라서 콘솔에 출력되는 순서는 A-C-B 순이다.

<br>

### 13 - Intro to SQL

