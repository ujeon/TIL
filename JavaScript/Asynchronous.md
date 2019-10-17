## 자바스크립트 비동기

### Synchronous vs Asynchronous

동기는 코드를 한 줄 한 줄 실행해 나가지만, 비동기는 그렇지 않죠. 아래의 코드를 살펴봅시다:

```javascript
// Synchronous
const second = () => {
  console.log('How are you doing?');
};

const first = () => {
  console.log('Hey there!');
  second();
  console.log('The end');
};

first();
/*
Hey there!
How are you doing?
The end
*/


// Asynchronous
const second = () => {
  setTimeout(() => {
    console.log('Async Hey There');
  }, 2000);
};

const first = () => {
  console.log('Hey there!');
  second();
  console.log('The end');
};

first();
/*
Hey there!
The end
How are you doing?
*/
```

결과가 나타나는 순서가 같을 것 같지만, 그렇지 않아요! 

코드가 한 줄 한 줄 순서대로 실행되지 않는다는 것은 이런 것을 의미합니다.

</br>

###자바스크립트 비동기 이해하기 : Event loop

앞서 살펴본 예제에서 만약 setTimeout이 2초 동안 기다렸다가 나머지 코드를 실행하였다면 결과는 동기와 같았을 것입니다. 그런데 기다리지 않고 다음 코드를 바로 실행한 이유가 무엇일까요?



다음의 경우를 예시로 살펴봅시다:

```javascript
const image = document.getElementById('img').src;

processLargeImage(image, () => {
  console.log('Image processed!');
});
```

위의 예시에서 이미지가 처리되는 시간이 어느 정도 필요할 것입니다. 만약 이미지가 처리 될 때 까지 다음 실행이 기다리고 있으면 어떻게 될까요? 아마도 그런 상황은 아무도 원하지 않을 거에요.



앞서 사용된 비동기 예시를 가지고 비동기가 어떻게 실행되는지, 이벤트 루프가 무엇인지 알아봅시다.

```javascript
// Asynchronous
const second = () => {
  setTimeout(() => {
    console.log('Async Hey There');
  }, 2000);
};

const first = () => {
  console.log('Hey there!');
  second();
  console.log('The end');
};

first();
```

1. first 함수를 호출하면 콜 스택에 first 함수의 실행 컨택스트가 생성됩니다.

2. first 함수의 첫 번째 코드인 console.log가 콜 스택에 푸쉬되고 'Hey there!'을 콘솔에 출력한 뒤 

​        console.log는 콜 스택에서 빠져나오게 됩니다.

3. 다음으로 두번째 코드인 second 함수가 호출되고 콜 스택에 쌓인다. 이어서 setTimeout 함수가 호출되는데, 이 

​       함수는 DOM 이벤트처럼 웹 API의 한 종류입니다. 해당 영역에 2초 후에 실행되는 타이머를 설정한 후   

​       console.log(콜백 함수)를 그 타이머에 묶어둡니다. 그런 다음 setTimeout은 콜 스택에서 빠져나오게 됩니다.

4. second 함수는 실행이 끝났으므로 콜 스택에서 빠져나오며, 이어서 세번째 코드인 console.log가 콜 스택에 

​       푸쉬되고 콘솔에 'The end'를 출력한 뒤 콜 스택에서 빠져나옵니다.

5. 세번 째 코드까지 모두 실행되었으므로 first 함수의 실행 컨택스트 역시 콜 스택에서 빠져나오게 되며, 콜 스택

​       에는 글로벌 실행 컨택스트만 남게 됩니다. 

6. 2초가 지나면 setTimeout의 콜백 함수를 묶고 있던 타이머가 사라지고, 콜백 함수가 메세지 큐에 대기하게 됩니다.

7. 이제 이벤트 루프가 할 일만 남았습니다. 이벤트 루프는 콜 스택과 메세지 큐를 살펴보다가 콜 스택이 비어있으면 

​       메세지 큐에 대기해 있는 것을 불러와 콜 스택에 푸쉬하는 역할을 합니다. 따라서 콜 스택에 console.log가 푸쉬  

​       되며 콘솔에 'Async Hey There'를 출력한 뒤 콜 스택에서 빠져나오게 됩니다. 이로써 모든 실행이 끝나게 됩니다.

</br>

###오래된 방법 : 콜백을 사용하여 자바스크립트 비동기 다루기

오래된 방법인 콜백을 사용하여 자바스크립트 비동기를 다루는 방법입니다.

```javascript
function getRecipe() {
  setTimeout(() => {
    const recipeID = [523, 823, 432, 947];
    console.log(recipeID);
    
    setTimeout(id => {
      const recipe1 = {title : '초간단 김치찌개', publisher : '백종원'};
      console.log(`${id} : ${recipe1.title}`);
      
      setTimeout(publisher => {
        const recipe2 = {title : '차돌박이 된장찌개', publisher : '백종원'};
        console.log(recipe2);
      }, 1500, recipe1.publisher);
    }, 1500, recipeID[2]);
  }, 1500);
};
```

콜백 함수 안에 함수를 호출하고 다시 함수를 호출하고 또 다시 함수를 호출하고 ... 호출해야 할 함수가 많아질수록 더 읽기 힘들어지고 복잡해집니다.

이러한 문제를 해결하기 위해서 자바스크립트는 프로미스라는 것을 제공합니다. 프로미스에 대해서는 이어서 살펴보도록 하겠습니다.

</br>

###콜백 헬에서 빠져나오기 :  promise

프로미스는 비동기와 관련된 객체입니다. 프로미스는 다음과 같은 것들을 포함하고 있습니다.

1. 특정한 이벤트가 이미 실행되었는지 아닌지를 추적하는 객체입니다.

2. 그 이벤트가 실행되었다면, 그 이후에 무엇이 일어날지 결정합니다.

3. 기대하는 미래 가치 개념을 구현합니다. (무슨 말일까요..?)



비동기는 시간과 관련되어 코드를 다루기 때문에 프로미스는 다른 상태를 가질 수 있습니다. 예를 들어, 어떤 이벤트가 발생하기 전에는 프로미스의 상태가 pending입니다. 이어서 이벤트가 발생하게 되면 프로미스의 상태는 settled 혹은 resolved가 됩니다. (둘 다 같은 의미입니다.)

settled 혹은 resolved인 상태에서 프로미스의 결과가 성공적이고 사용가능한 상태라면 프로미스는 fufilled 되지만, 그렇지 않다면 rejected 됩니다.



이제 프로미스를 사용하여 이전의 코드를 다시 만들어 봅시다.

```javascript
const getIDs = new Promise((resolve, reject) => {});
```

new 키워드를 사용하여 프로미스를 생성한 다음, 생성 직후에 실행할 함수를 삽입합니다. 이 함수는 프로미스에게 이벤트의 결과가 성공적인지 아닌지를 알려주는 데 사용됩니다. 그리고 함수는 두 개의 파라미터를 가지는데, 하나는 프로미스가 resolve인 상태일 때 사용할 콜백 함수이고, 다른 하나는 reject일 때 사용할 콜백 함수입니다.



이어서 프로미스가 fufilled 될 때 resolve 함수를 사용하는 경우를 구현해보겠습니다. (setTimeout 함수는 서버로 부터 정보를 받아오는 역할을 하는 것으로 가정하겠습니다.)

```javascript
const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([523, 823, 432, 947]); // resolve의 인자는 성공적으로 실행된 프로미스입니다.
  }, 1500);
});
```

resolve에 프로미스를 인자로 전달하면, 프로미스가 fulfilled 된 것으로 표시하고, 프로미스를 이행하게 됩니다.



프로미스를 사용할 때는 then, catch 두가지 메서드를 이용합니다.

먼저 then 메서드는 프로미스가 fufilled인 경우 이벤트 핸들러를 추가할 수 있도록 합니다. then 메서드는 하나의 파라미터를 가질 수 있는데, 이 파라미터는 성공한 프로미스의 결과를 then이 인자로 받을 수 있도록 합니다. 즉, then은 성공한 프로미스의 결과를 전달 받습니다.



이와는 반대로 프로미스가 rejected 된 경우에는 catch 메서드를 사용합니다.

```javascript
const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([523, 823, 432, 947]);
  }, 1500);
});

getIDs
.then(IDs => {
  console.log(IDs); // [523, 823, 432, 947]
})
.catch(error => {
  console.log(error) // 프로미스가 rejected 된 경우 에러를 표시합니다.
});
```

위의 예제에서 setTimeout은 언제나 성공적인 프로미스를 반환하므로 catch 메서드가 실행되는 경우는 없죠.



계속해서 코드를 작성해보겠습니다.  

```javascript
const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([523, 823, 432, 947]);
  }, 1500);
});

const getRecipe = recID => {
  return new Promise((resolve, reject) => {
    setTimeout(ID => {
      const recipe = {title : '초간단 김치찌개', publisher : '백종원'};
      resolve(`${ID} : ${recipe.title}`);
    }, 1500, recID);
  });
};

const getRelated = publisher => {
  return new Promise((resolve, reject) => {
    setTimeout(pub => {
      const recipe = {title : '차돌박이 된장찌개', publisher : '백종원'};
      resolve(`${pub} : ${recipe.title}`);
    }, 1500, publisher);
  });
};

getIDs
.then(IDs => {
  console.log(IDs); // [523, 823, 432, 947]
  return getRecipe(IDs[2]);
})
.then(recipe => {
  console.log(recipe); // 432 : 초간단 김치찌개
  return getRelated('백종원');
})
.then(recipe => {
  console.log(recipe); // 백종원 : 차돌박이 된장찌개
})
.catch(error => {
  console.log(error) // 프로미스가 rejected 된 경우 에러를 표시합니다.
});
```

프로미스를 사용하지 않을 때와 같은 결과를 출력하는 코드가 완성되었습니다.

</br>

### promise에서 async / await로 넘어가기

프로미스가 콜백 함수를 반복해서 사용하는 것보다는 편리하긴 하지만, 여전히 불편한 점이 존재합니다. ES7에서는 단점을 개선하기 위해서 async 와 await를 제공합니다. async 와 await는 프로미스를 생성하는 것이 아니라 사용하기 위해서 디자인 되었습니다. 프로미스를 생성하기 위해서는 이전의 방법을 사용해야 하죠.

이번에는 async 와 await를 사용해서 이전에 작성하였던 다음의 코드를 다시 작성해보도록 하겠습니다.

```javascript
getIDs
.then(IDs => {
  console.log(IDs); // [523, 823, 432, 947]
  return getRecipe(IDs[2]);
})
.then(recipe => {
  console.log(recipe); // 432 : 초간단 김치찌개
  return getRelated('백종원');
})
.then(recipe => {
  console.log(recipe); // 백종원 : 차돌박이 된장찌개
})
.catch(error => {
  console.log(error) // 프로미스가 rejected 된 경우 에러를 표시합니다.
});
```

```javascript
const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([523, 823, 432, 947]);
  }, 1500);
});

const getRecipe = recID => {
  return new Promise((resolve, reject) => {
    setTimeout(ID => {
      const recipe = {title : '초간단 김치찌개', publisher : '백종원'};
      resolve(`${ID} : ${recipe.title}`);
    }, 1500, recID);
  });
};

const getRelated = publisher => {
  return new Promise((resolve, reject) => {
    setTimeout(pub => {
      const recipe = {title : '차돌박이 된장찌개', publisher : '백종원'};
      resolve(`${pub} : ${recipe.title}`);
    }, 1500, publisher);
  });
};

async function getRecipeAW() {
  const IDs = await getIDs; // 성공적인 프로미스를 변수 IDs에 저장합니다.
  console.log(IDs); // [523, 823, 432, 947]
  const recipe = await getRecipe(IDs[2]);
  console.log(recipe); // 432 : 초간단 김치찌개
  const related = await getRelated('백종원');
  console.log(related); // 백종원 : 차돌박이 된장찌개

  return recipe;
};

getRecipeAW(); // 프로미스를 반환합니다.
```

asycn는 비동기 함수를 표준 동기 함수 작성과 비슷하게 만들어 주는 역할을 합니다. 또한 코드 안의 내용을 실행한 후에 암시적으로 프로미스를 사용하여 결과를 반환합니다. 즉, 프로미스를 리턴합니다.

반면, await의 역할은 비동기 실행이 진행되는 동안 async 함수를 정지 하는 것 입니다. 그리고 await는 async 안에서만 동작합니다.



async/await함수의 목적은 사용하는 여러 프로미스의 동작을 동기스럽게 사용할 수 있게 하고, 어떠한 동작을 여러 프로미스의 그룹에서 간단하게 동작하게 만드는 것 입니다. 



아래의 코드는 작동할까요?

```javascript
const recipe = getRecipeAW();
console.log(recipe); // Promise {<pending>} => 이벤트가 일어나기 전 상태임을 알 수 있습니다.
```

물론 작동하지 않습니다. 왜냐하면 비동기 함수 getRecipeAW가 작동하기 전에 동기 함수 console.log가 먼저 작동하기 때문이죠. 위의 코드를 정상적으로 작동하게 만들기 위해서는 다음과 같이 작성해야 합니다.

```javascript
getRecipeAW().then(result => console.log); // 432 : 초간단 김치찌개
```



지금까지 자바스크립트 비동기에 대해 알아보았습니다.

감사합니다.