# Closure

## 클로저란?
- 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것을 말함
- 함수 내에서 함수를 정의한 경우
- 클로저 안에 정의된 함수는 만들어진 환경을 기억
  
```javascript
function getClosure() {
  var text = 'variable 1';
  return function() {
    return text;
  };
}

var closure = getClosure();
console.log(closure()); // 'variable 1'
```
    - getClosure()는 함수를 반환하고 반환된 함수는 getClosure() 내부에서 선언된 변수를 참조. 참조된 변수는 함수 실행이 끝났다고 해서 사라지지 않고 제대로 값을 반환하고 있다.
    - 즉, 외부함수의 지역변수 text가 소멸되지 않았다는 것을 의미
    - 클로저란 내부함수가 외부함수의 지역변수에 접근 할 수 있고, 외부함수는 외부함수의 지역변수를 사용하는 내부함수가 소멸될 때까지 소멸되지 않는 특성을 의미함.

```javascript
var base = 'Hello, ';
function sayHelloTo(name) {
  var text = base + name;
  return function() {
    console.log(text);
  };
}

var hello1 = sayHelloTo('aaa');
var hello2 = sayHelloTo('bbb');
hello1(); // 'Hello, aaa'
hello2(); // 'Hello, bbb'
```
    - 결과를 보면 text 변수가 동적으로 변화하고 있는 것처럼 보이지만 실제로는 text라는 변수가 여러번 생성된 것. 
    - 즉, hello1과 hello2는 서로 다른 환경을 가지고 있음

## 클로저를 통한 은닉화
```javascript
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function() {
  console.log('Hello, ' + this._name);
}

var hello1 = new Hello('aaa');
var hello2 = new Hello('bbb');

hello1.say(); // 'Hello, aaa'
hello2.say(); // 'Hello, bbb'
hello1._name = 'anonymous';
hello1.say(); // 'Hello, anonymous'
```
    - 자바스크립트 네이밍 컨벤션을 생각해봤을 때 _name 변수는 Private variable으로 쓰고싶다는 의도를 알 수 있다. 
    - 하지만 실제로는 여전히 외부에서도 쉽게 접근가능한 변수일 뿐
  
```javascript
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('aaa');
var hello2 = hello('bbb');

hello1(); // 'Hello, aaa'
hello2(); // 'Hello, bbb'
```
    - 클로저 사용 시 은닉화가 가능하다

## 반복문 클로저
```javascript
var i;
for (i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i); // 10 열 번 출력
  }, 100);
}
```
    - 0.1초 동안 이미 반복문이 모두 순회되면서 i값이 이미 10이 된 상태. 그 때 익명함수가 호출되면서 이미 10이 돼버린 i를 참조

```javascript
var i;
for (i = 0; i < 10; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j); //1 2 3 ... 10
    }, 100);
  })(i);
}
```
    - 이 코드에서 i는 IIFE(즉시 실행 함수) 내에 j라는 형태로 주입되고 클로저에 의해 각기 다른 환경 속에 포함됨.

## 클로저의 성능
```javascript
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('aaa');
var hello2 = hello('bbb');

hello1(); // 'Hello, aaa'
hello2(); // 'Hello, bbb'

// 여기서 메모리를 release 시키기 클로저의 참조를 제거해야 한다.
hello1 = null;
hello2 = null;
hello3 = null;
```
    - 클로저가 각자의 환경을 가지는만큼 환경을 기억하기 위한 메모리가 소모
    - 따라서 클로저 사용이 끝나면 참조를 제거하는 것이 좋음

###### reference https://hyunseob.github.io/2016/08/30/javascript-closure/ 