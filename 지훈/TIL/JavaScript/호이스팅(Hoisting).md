# 호이스팅(Hoisting)

## 정의

<pre>
변수나 함수의 scope의 시작 지점에서 해당 식뱔자의 관측이 가능한 현상
</pre>

호이스팅 발생 원인은 자바스크립트의 코드 실행 과정에 있다.

## 자바스크립트 코드 실행 과정

1. 소스코드 평가
   - 선언문으로 작성된 변수와 함수 등을 실행하여 변수와 함수 식별자를 키로 하여 실행 컨텍스트가 관리하는 환경에 등록
2. 소스코드 실행
   - 소스코드가 순차적으로 샐행, 이때 변수나 함수를 만나면 실행 컨텍스트가 관리하는 환경에서 검색하여 참조

## 변수 호이스팅

`var`는 호이스팅 되어 선언됨과 동시에 `undefined`로 초기화 되어서 scope 어디서든 참조가 가능하다.

`let`과 `const`에서도 호이스팅은 일어나지만 선언만 할뿐 초기화는 이루어지지않기 때문에 초기화 이후에 참조가능하다.

아래의 예제를 통해 변수 호이스팅에 대해 알아보자.

```js
console.log(value);
```

출력 결과

```
ReferenceError: value is not defined
```

선언되어있지 않기 때문에 에러가 발생하는 것이 당연하다.
그렇다면 `console.log(value)`이후에 value를 선언하면 어떻게 될까?

```js
console.log(value);
var value = 5;
```

출력 결과

```
undifined
```

ReferenceError 가 아닌 undifined 가 출력되는 것을 볼 수 있다. 호이스팅에 의해 아래와같이 코드가 인식되었기 때문이다.

```js
var value = undefined;
console.log(value);
value = 5;
```

선언은 되어있지만 초기화는 되어있지 않기 때문에 undefined 나오는 것이다.

아래의 결과를 예측해보자

```js
var value = 123;
function func() {
  console.log(value);
  var value = 456;
  console.log(value);
}
func();
```

```
123
456
```

이 나올것 같기도하고

```
456
456
```

이 나올것 같기도하다

하지만 결과는

```
undefined
456
```

[정의](#정의)를 보면 이해할 수 있는데 위 코드가 호이스팅 되면 아래와 같이 인식된다.

```js
var value = undefined;
value = 123;
function func() {
  var value = undefined;
  console.log(value);
  value = 456;
  console.log(value);
}
func();
```

var의 scope 가 function 이기때문에 funtion의 최상단에 다시 선언되었기 때문에 이러한 결과가 나온것이다.

## 함수 호이스팅

함수 호이스팅에 대해 알아보기에 앞서 함수 표현식과 함수 선언식에 대해 알아야한다.

### 함수 표현식

자바스크립트 함수는 표현식을 사용하여 정의 될 수 있으며, 함수 표현식은 변수로 저장될수 있다.

```js
var func = function () {
  return "이것은 함수입니다.";
};
```

함수 표현식이 변수에 저장되면, 변수는 함수처럼 사용 가능해진다. 변수에 저장된 함수는 함수명이 필요 없으며, 변수 이름을 통하여 호출된다.

### 함수 선언식

번수 선언이 `var, let, const`로 시작하는것 처럼 `function`으로 시작하는것이 함수 선언식이다.

```js
function func() {
  return "이것은 함수입니다.";
}
```

사용하려면 해당 함수 이름(`func`)을 호출하면 된다.

아래의 예제를 통해 함수 호이스팅에 대해 알아보자.

### 1. var 함수 표현식

```js
count();

var count = function () {
  console.log("count는 1이다.");
};
```

출력결과

```
TypeError: "count" is not a function
```

함수 표현식으로 되어있어 함수같아 보이지만 변수 `count` 에 담겨 있으므로 변수 호이스팅 규칙을 따른다 따라서 코드가 다음과 같이 인식된다

```js
var count = undefined;

count();

count = function () {
  console.log("count는 1이다.");
};
```

아래에 함수 초기화 부분을 만나기 전까지는 undefined 변수 이므로 `count()`은 TypeError가 발생한다.

### 2. let, const 함수 표현식

```js
count();

let count = function () {
  console.log("count는 1이다.");
};
```

출력결과

```
ReferenceError: count is not defined
```

미찬가지로 변수 호이스팅 규칙을 따르므로 정의 되지않은 함수를 호출했기 때문에 ReferenceError가 발생한다.

### 3. 함수 선언식

```js
count();

function count() {
  console.log("count는 1이다.");
}
```

출력결과

```
count는 1이다.
```

함수 표현식과 달리 함수 선언식은 선언부 뿐만 아니라 함수 전체가 실행 컨텍스트에 등록되기 때문이다.

함수 선언식의 문제점

```js
function count() {
  console.log("count는 1이다.");
}
count();

function count() {
  console.log("count는 2이다.");
}
count();
```

출력결과

```
count는 2이다.
count는 2이다.
```

함수 선언식 호이스팅 규칙에 따라 위의 코드는 다음과 같이 인식된다

```js
function count() {
  console.log("count는 1이다.");
}
function count() {
  console.log("count는 2이다.");
}

count();
count();
```

같은 이름으로 선언이 가능하기때문에 둘다호이스팅 되어 마지막에 선언한 함수로만 실행된것을 확인할 수 있다.
