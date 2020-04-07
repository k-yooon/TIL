# Arrow Function

## 화살표 함수
- ES6 문법
- function 표현에 비해 구문이 짧음 
- 자신의 this, arguments, super 또는 new.target을 바인딩 하지 않음

```javascript
// 일반 함수
var foo = function () { console.log("foo") }; // foo

// 화살표 함수
var bar = () => console.log("bar"); // bar

// 매개변수가 없는 경우
var foo = () => console.log('bar');
foo(); // bar

// 매개변수가 하나인 경우
var foo = x => x;
foo('bar'); // bar

// 매개변수가 여려개인 경우
var foo = (a, b) => a + b; // 간단하게 한줄로 표현할 땐 "{}" 없이 값이 반환됩니다.
foo(1, 2); // 3

var foo = (a, b) => { return a + b }; 
foo(1, 2); // 3

var foo = (a, b) => { a + b }; // "{}"를 사용했는데 return이 없을 때 
foo(1, 2); // undefined

var foo = (a, b) => { // 여러줄 썼을 때
	var c = 3;
	return a + b + c;
}
foo(1, 2, 3) // 6
/*
"{}"를 사용하면 값을 반환할 때 return을 사용해야합니다.
"{}"를 사용하지 않으면 undefied를 반환합니다.
*/

// 객체를 반환할 때
var foo = () => ( { a: 1, b: 2, c: 3 } );
foo(); // { a: 1, b: 2, c: 3 };

// 콜백함수에서 사용할 때
var numbers = [1, 4, 9];
var oddArr = numbers.filter( x => (x % 2) !== 0 );
console.log(oddArr); // [1, 9]
```

## 화살표 함수 this
- 일반함수는 전역 컨텍스트에서 실행될 때 this가 정의 (함수의 내부함수, 콜백함수에 사용되는 this는 window). 따라서 바인딩 팔요.
  
```javascript
let cat = {
	sound: "meow",
  	soundPlay: function () {
      	console.log(this) // 가.
		  setTimeout(function () {
			console.log(this) // 나.
		}, 1000);
    }
}

cat.soundPlay();
// 가. cat {sound: "meow", soundPlay: ƒ}
// 나. window

//bind 사용
let cat = {
	sound: "meow",
  	soundPlay: function () {
		setTimeout(function () {
			console.log(this.sound);
		}.bind(this), 1000); // bind 사용
    }
}

cat.soundPlay(); // meow

//화살표 함수
let cat = {
	sound: "meow",
  	soundPlay: function () {
		setTimeout(() => {
			console.log(this.sound);
		}, 1000);
    }
}

cat.soundPlay(); // meow
```

###### references https://velog.io/@ki_blank/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98Arrow-function
