# Promise

## Promise란?

- 프로미스는 자바스크립트 비동기 처리에 사용되는 객체
- 프로미스 메서드를 호출할 때 콜백 함수의 인사로 resolve, reject에 접근 가능

```
new Promise(function (resolve, reject) {

});
```

### Promise 상태

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
  - new Promise() 메서드 호출 시
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
  - resolve 실행 시
  - 성공 시 처리 결과를 then으로 받을 수 있음
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태
  - reject 실행 시
  - 실패 시 처리 결과를 catch로 받을 수 있음

### 예제

```
const promise = new Promise( function(resolve, reject){
  if(condition){
    resolve('성공');
  } else {
    reject('실패');
  }
});

promise.then(function(message) {
  console.log(message);
}).catch(function(error) {
  console.error(error);
});
```

## Promise.all

- 여러 개의 비동기 작업들이 존재하고 이들이 모두 완료됐을 때 작업을 진행하고 싶다면 Promise.all

### 예제

```
const promise1 = Promise.resolve('성공1');
const promise2 = Promise.resolve('성공2');
Promise.all([promise1, promise2])
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.error(error);
  });
```

##### reperence https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
