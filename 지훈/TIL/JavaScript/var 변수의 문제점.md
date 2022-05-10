# var 변수의 문제점

## 1. 같은 이름의 변수를 여러번 선언가능하다.

```js
var foo = 123;
console.log(foo);
var foo = 456;
console.log(foo);
```

출력 결과

```bash
123
456
```

위의 코드와 같이 foo 라는 이름의 변수를 두번 선언했지만 문제없이 작동한다.

let 과 const 는 동일한 이름의 변수는 한번만 선언 가능하다.

```js
const foo = 123;
console.log(foo);
const foo = 456;
console.log(foo);
```

출력 결과

```
123
SyntaxError: Identifier 'foo' has already been declared
```

## 2. var는 function 단위의 scope를 가진다.

scope 문제를 이해하기 위해 우선 호이스팅(Hoisting)에 대해서 알아야한다.

### 호이스팅(Hoisting) [[자세히 알아보기](<호이스팅(Hoisting).md>)]

변수나 함수의 scope의 시작 지점에서 해당 식뱔자의 관측이 가능한 현상

```js
function scopreTest() {
  var arr = [];
  for (var i = 0; i < 5; i++) {
    arr[i] = function () {
      console.log(i);
    };
  }
  arr[3];
}
```

위 함수의 출력값을 예상해보면 3이 나올것 같지만 출력 결과는 5이다.

그 이유는 반복문 안에 선언된 i가 function 단위 scope 를 가지기때문에 호이스팅 되었을때 함수 최상단에서 관측가능하기 때문이다

위의 코드는 아래의 코드와 동일하게 동작한다.

```js
function scopreTest() {
  var arr = [];
  var i = undefined;
  for (i = 0; i < 5; i++) {
    arr[i] = function () {
      console.log(i);
    };
  }
  arr[3];
}
```

var의 scope가 function 이기때문에 함수 최상단으로 호이스팅 되어 문제가 발생한 것이다.

let 과 const 는 block 단위 scope이기 때문에 위와같은 문제가 발생하지 않는다.
