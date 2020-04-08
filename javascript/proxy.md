# Proxy

## Proxy란?
- 대리해주는 객체
- 자바스크립트에서의 기본 작업, 예를들면 속성 조회, 할당, 열거, 함수 호출 등에 대한 행위에 대해 사용자의 커스텀 동작을 정의할 때 사용할 수 있음
- 동작 정의에는 get, set, has, defineProperty, deleteProperty, construct, apply 등 13가지 함수가 있음.
  
```javascript
const proxy = new Proxy(target, handler);
```
    - target에는 Proxy로 랩핑할 대상 지정(배열, 함수, proxy 객체 등)
    - handler에는 operation이 수행될 때 proxy의 동작을 정의하는 함수 객체를 넣어줌(조회, 할당, 열거 함수 호출 등)

## handler
### set / get
```javascript
const myObj = {name : 'crong', changedValue : 0};
const proxy = new Proxy(myObj, {
  get : function(target, property, receiver){
    console.log('get value');
    
    return target[property];
  },
  set : function(target, property, value){
    console.log('set value');
    target['changedValue']++;
    target[property] = value;
  }
})
```
    - proxy.name = 'js'시 자동 set 함수 호출
    - proxy.name시 자동 get 함수 호출

### construct
```javascript
var p = new Proxy(function() {}, {
construct: function(target, argumentsList, newTarget) {
console.log('called: ' + argumentsList.join(', '));
return { value: argumentsList[0] * 10 };
}
});

console.log(new p(1, 2, 3).value); //10
```
    - new 연산자 실행시 자동 construct 함수 호출
  
