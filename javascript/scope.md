# scope

## scope란 
- 변수에 접근할 수 있는 범위 
  
### 전역 스코프
- 전역에 선언되어 있어 어느 고에서든지 해당 변수에 접근할 수 있다는 의미 

### 지역 스코프
- 해당 지역에서만 접근할 수 있어 지역을 벗어난 곳에선 접근할 수 없다는 의미
- 함수에서 선언한 변수는 해당 함수에서만 접근할 수 있음. 이걸 함수 스코프라고 함. 함수 스코프가 지역 스코프의 예

```javascript
var x = 'global';
function ex() {
  x = 'change';
}
ex();
alert(x); // 'change'
```

## 스코프 체인
- 변수를 찾을 때 범위를 넓히면서 찾는 관계를 스코프 체인이라고 함
  
```javascript
var name = 'zero';
function outer() {
  console.log('외부', name);
  function inner() {
    var enemy = 'nero';
    console.log('내부', name);
  }
  inner();
}
outer();
console.log(enemy); // undefined
```
    - inner 함수는 name 변수를 찾기 위해서 먼저 자기 자신의 스코프에서 찾고 없으면 한 단계 올라가 outer 스코프에서 찾음

## 정적 스코프
- 스코프는 함수를 호출할 때가 아닌 선언할 때 생김
- 함수를 선언 하는 순간, 함수 내부의 변수는 자기 스코프로부터 가장 가까운 곳(상위 범위에서)에 있는 변수를 계속 참조하게 됨
- 따라서 변수 선언시 전역변수를 만드는 일은 지양하는 것이 좋음

```javascript
var name = 'zero';
function log() {
  console.log(name);
}

function wrapper() {
  name = 'nero';
  log();
}
wrapper(); //nero

var name = 'zero';
function log() {
  console.log(name);
}

function wrapper() {
  var name = 'nero';
  log();
}
wrapper(); //zero
```
    - 두번째 예시에서 log 함수 안의 name 변수는 선언 시 가장 가까운 전역변수 name을 참조. 따라서 wrapper 안에서 log를 호출해서 지역변수가 아니라 전역변수의 값이 나옴


###### reference https://medium.com/@yeon22/javascript-%EC%8A%A4%EC%BD%94%ED%94%84-scope-%EB%9E%80-bc761cba1023