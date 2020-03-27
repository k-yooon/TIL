# Prototype Methods VS Object Methods

## Method 선언 방법

1. 객체 생성자 내부에서 this.func = function(){}
2. Obj.prototype.func = function(){}
   
** 생성자 함수에 추가하는 메소드의 용도가 객체 인스턴스들이 공통으로 사용하는 함수를 정의하는 것이라면 prototype을 사용하는 것이 좋음(변경 용의 / 속도 / 메모리 효율)
  
## Object Methods
```javascript
function Animal(animal) {
    this.animal = animal;
  
    // 객체 생성자 내에서 모든 객체 인스턴스에서 공용으로 쓰이는 메소드를 추가합니다.
    this.bark = function(cry) {
      console.log(this.animal + " : " + cry);
    };
  }
  
  var dog = new Animal("dog");
  var cat = new Animal("cat");
  dog.bark("walwal"); // dog : walwal
  cat.bark("mew"); // cat : mew
  
  Animal.bark = function() {
    console.log(`${this.animal} : kkk...`);
  };
  
  dog.bark(); // dog : undefined
  cat.bark(); // cat : undefined
  
  
  var cow = new Animal("cow");
  cow.bark(); // cow : undefined
  
  
  dog.bark = function() {
    console.log("dog : kkk...");
  };
  cat.bark = function() {
    console.log("cat : kkk...");
  };
  dog.bark(); // dog : kkk...
  cat.bark(); // cat : kkk...
```
- 이미 인스턴스는 생성되었기 때문에 공용 메소드를 변경하지 못하고 일일히 인스턴스를 변경해야함.

## Prototype Methods
```javascript
function Animal(animal){
    this.animal = animal;
}

Animal.prototype.bark = function(cry){
    console.log(this.animal + " : " + cry);
}

let dog = new Animal("dog");
let cat = new Animal("cat");
dog.bark("walwal"); // dog : walwal
cat.bark("mew"); // cat : mew

Animal.prototype.bark = function(){
    console.log(`${this.animal} : kkk...`);
}

dog.bark(); // dog : kkk...
cat.bark(); // cat : kkk...

let cow = new Animal("cow");
cow.bark(); // cow : kkk...
```

###### reperence https://velog.io/@gtobio11/Javascript-Prototype-methods-vs-Object-methods