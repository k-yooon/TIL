# 필수 Array 메소드

## includes() 
- 배열의 요소 중에서 인자가 존재하는지 체크
```javascript
var arr = [1, 2, 3, 4, 5];
console.log(arr.includes(3)); //true
console.log(arr.includes(6)); //false
```

## filter() 
- 콜백 함수의 조건을 통과한 요소들만 모아서 새로운 배열 생성
```javascript
var filterd = arr.filter(num => num > 3);
console.log(filterd); //[4, 5]
```

## map()
- 배열의 모든 요소를 인자로 넘긴 후 도출된 결과값들을 모아 새로운 배열 생성
```javascript
var mapArr = arr.map(num => num + 1);
console.log(mapArr); //[2, 3, 4, 5, 6]
```

## reduce() 
- 인자를 왼쪽부터 차례대로 누산하여 하나의 값으로 축소
```javascript
var sum = arr.reduce((total, value) => total + value, 0);
console.log(sum);  //15
```

## some() 
- 배열의 요소 중 적어도 한 개 이상이 콜백함수의 조건을 만족하는 경우 true
```javascript
var largeNum = arr.some(num => num > 4);
console.log(largeNum);  //true
var smalllNum = arr.some(num => num <= 0);
console.log(smalllNum); //false
```

## every() 
- 모든 배열 요소가 콜백함수의 조건을 만족하면 true
```javascript
var greaterFour = arr.every(num => num > 4);
console.log(greaterFour); //false
```

## sort()
- 배열 요소를 sort 
```javascript
var descOrder = arr.sort((a, b) => a > b? -1 : 1); //내림차순
console.log(descOrder); //[5, 4, 3, 2, 1]
var ascOrder = arr.sort((a, b) => a > b? 1 : -1); //오름차순
console.log(ascOrder); //[1, 2, 3, 4, 5]
```

## Array.from() 
- 배열과 유사하거나 반복적인 형태를 가지고 있는 요소를 배열로 만들어줌
```javascript
var name = 'javascript';
var nameArr = Array.from(name);
console.log(nameArr); //['j', 'a', 'v', 'a', 's', 'c', 'r', 'i' 'p', 't']

//전개 문법으로도 동일 기능 구현 가능
var nameArr2 = [...name];
console.log(nameArr2); //['j', 'a', 'v', 'a', 's', 'c', 'r', 'i' 'p', 't']
```

## Array.of() 
- 모든 요소를 모아 새로운 배열 생성
```javascript
var nums = Array.of(10, 9, 8, 7);
console.log(nums); //[10, 9, 8, 7]
```