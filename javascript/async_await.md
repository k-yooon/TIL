# async / await

## async / await란?

- 콜백과 프로미스의 단점을 보완한 문법
- 예외 처리는 try catch 이용

### 예제

- callback

```javascript
function findAndSaveUser(Users) {
  Users.findOne({}, function(err, user) {
    if (err) {
      return console.error(err);
    }
    user.name = 'zero';
    user.save(function(err) {
      if (err) {
        return console.error(err);
      }
      Users.findOne({ gender: 'm' }, function(err, user) {});
    });
  });
}
```

- 프로미스

```javascript
function findAndSaveUser(Users) {
  Users.findOne({})
    .then(function(user) {
      user.name = 'zero';
      return user.save();
    })
    .then(function(user) {
      return Users.findOne({ gender: 'm' });
    })
    .then(function(user) {})
    .catch(function(err) {
      console.error(err);
    });
}
```

- async / await

```javascript
async function findAndSaveUser(Users) {
  try {
    let user = await Users.findOne({});
    user.name = 'zero';
    user = await user.save();
    user = await User.findOne({ gender: 'm' });
  } catch (error) {
    console.error(error);
  }
}
```

###### reperence https://joshua1988.github.io/web-development/javascript/js-async-await/
