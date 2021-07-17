# 스코프(Scope)와 렉시컬스코핑(Lexical Scoping)

## 스코프(Scope)

스코프란 프로그램의 실행 컨텍스트(Execution Context)에서 보거나 접근할 수 있는 범위를 뜻한다.

### 전역 스코프

함수 바깥에 선언되어 있는 변수는 전역 스코프에 포함된다.

```js
var global = "전역변수";

function isGlobal() {
  return `${global}`는 전역에서 접근가능하다.
}

isGlobal(); // 전역변수는 전역에서 접근가능하다.
```

global 함수 내에서 `global` 변수가 없어도 정상적으로 global을 참조하여 함수가 실행된다. 이는 **스코프 체인**에 의해 일어나는 현상이다. 현재 자신의 스코프에서 사용하고자 하는 변수가 없다면 스코프 체인을 통해 상위 스코프에서 변수를 찾는다.

전역 스코프는 어디에서나 접근 가능한 장점이 있지만, 다음과 같이 기존의 값이 변경되거나 하는 의도치 않은 오류가 발생할 수 있다.

```js
let x = 50;

function change() {
  x = 100;
  console.log(x);
}

change(); // 100
console.log(x); // 100
```

이러한 실수를 방지하기위해 전역 스코프에 변수를 선언하기보다, 지역 스코프에 변수를 선언하고 사용한다.

### 지역 스코프

지역 스코프는 해당 지역에서만 (혹은 그 내부) 접근할 수 있어 지역을 벗어난 곳에서 접근할 수 없다. 자바스크립트에서 지역 스코프는 크게 `함수 스코프`와 `블록 스코프`가 있다.

**함수 스코프**

함수 내부에서만 접근이 가능하다. 대표적으로 `var` 키워드로 선언된 변수가 함수 스코프를 가진다.

```js
function check() {
  var function_scope = "hi";
  console.log(`함수안 ${function_scope}`);
}

console.log(function_scope); // ReferenceError
check(); // 함수 안 hi
```

**블록 스코프**

블록 `{ }` 내에서만 접근 가능하다. 대표적으로 `let`, `const` 키워드로 선언된 변수가 블록 스코프를 가진다.

```js
function check() {
  if (true) {
    var function_scope = "hi var";
    let block_scope1 = "hi let";
    const block_scope2 = "hi const";

    console.log(`블록 안 ${function_scope}`); // 블록 안 hi var
    console.log(`블록 안 ${block_scope1}`); // 블록 안 hi let
    console.log(`블록 안 ${block_scope2}`); // 블록 안 hi const
  }
  console.log(`블록 밖 ${function_scope}`); // 블록 밖 hi var
  console.log(`블록 밖 ${block_scope1}`); // ReferenceError
  console.log(`블록 밖 ${block_scope2}`); // ReferenceError
}

check();
```

`var`의 경우 함수 스코프를 가지기에 블록 `{ }` 밖에서도 접근이 가능하지만, `let`, `const`의 경우 블록 스코프를 가지기에 블록 밖에서 접근이 불가능하다.

---

## 렉시컬 스코프(Lexical Scope)

렉시컬 스코프는 **함수가 선언**될 때 스코프가 결정된다는 것을 의미한다. 다른 말로 정적 스코프(Static Scope)라고도 한다.

```js
var x = 1; // global

function first() {
  var x = 10;
  second();
}

function second() {
  console.log(x);
}

first(); // 1
second(); // 1
```

위와 같은 결과가 나온 이유는, second 함수가 실행될 때 first 함수 내부에서 호출된 것과는 별개로 second 함수가 선언됐을 때 스코프가 결정되어 그 상위 스코프에 있는 `var x = 1;` 을 참조하기 때문이다.

```js
var x = 1; // global

function first() {
  x = 10; // <- 변경
  second();
}

function second() {
  console.log(x);
}

first(); // 1
second(); // 10
```

위 코드에선 first함수가 전역 변수를 건드려 `x = 10` 으로 변경시켰고, 그 전역변수를 second 함수가 참조하기에 이와 같은 결과가 나온다.
