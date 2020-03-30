# 비구조화 할당

## 배열
- 기존
```javascript
const animalList = ["CAT", "DOG", "TIGER"];
console.log(animalList[0]); // CAT
console.log(animalList[1]); // DOG
console.log(animalList[2]); // TIGER
```
- 비구조화 할당
```javascript
const [cat, dog, tiger] = ["CAT", "DOG", "TIGER"];
console.log(cat); // CAT
console.log(dog); // DOG
console.log(tiger); // TIGER
```

### 나머지 패턴 
```javascript
const animalList = ["CAT", "DOG", "TIGER"];
const [cat, ...restAnimalList] = animalList;
console.log(cat); // CAT
console.log(restAnimalList); // ["DOG", "TIGER"]
```

## 오브젝트
- 기존
```javascript
const animals = {
  cat: "CAT",
  dog: "DOG",
  tiger: "TIGER"
};
console.log(animals.cat); // CAT
console.log(animals.dog); // DOG
console.log(animals.tiger); // TIGER
```
- 비구조화 할당
```javascript
const { cat, dog, tiger } = {
  cat: "CAT",
  dog: "DOG",
  tiger: "TIGER"
};
console.log(cat); // CAT
console.log(dog); // DOG
console.log(tiger); // TIGER
```

### 나머지 패턴
```javascript
const { cat, ...animals } = {
  cat: "CAT",
  dog: "DOG",
  tiger: "TIGER"
};
console.log(cat); // CAT
console.log(animals); // { dog: DOG, tiger: TIGER }
```