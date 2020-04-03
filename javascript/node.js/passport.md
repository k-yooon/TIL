# passport 
#### http://www.passportjs.org/
- 인증관련 미들웨어
- 로그인 기능이나 특정 페이지에 접속을 위한 관리자 권한 등 인터셉터를 해주고 권한체크를 해줌
- express와 express-session 미들웨어를 연동함으로서 더 유연한 기능 제공

## 라이브러리 적용
### passport 초기화 및 Express-session 설정
```javascript
var passport = require('passport');
var session = require('express-session');

//Express-session 설정
app.use(session({
    secret : 'keyboard cat',
    resave : false, //변경 없어도 저장
    saveUninitialized : true //세션 초기화 전에도 저장
}));

//passport setting 
app.use(passport.initialize());
app.use(passport.session());
```
    - initialize를 이용해서 passport 초기화
    - session 이용해서 앞서 정의한 Express-session 정보와 연동

### 인증 기능 구현
```javascript
var LocalStrategy = require('passport-local').Strategy;

//회원가입
router.post('/', passport.authenticate('local-join', {
    successRedirect : '/main',
    failureRedirect : '/join',
    failureFlash : true
}))

//로그인
router.post('/', function(req, res, next){
   passport.authenticate('local-login', function(err, user, info){
       if(err) res.status(500).json(err);
       if(!user) return res.status(401).json(info.message);
       req.logIn(user, function(err){
           console.log(user);
           if(err) return next(err);
           return res.json(user);
       })
   })(req, res, next); 
})

//로그아웃
router.get('/', function(req, res){
    req.logout();
    res.redirect('/login');
})
```
  
### 전략 구성
- 로컬전략은 개발자가 직접 DB를 연동하여 인증로직을 구현
- passport 미들웨어 관련이므로 passport.use()로 구성
- 내부적으론 LocalStrategy를 생성해 전략에 필요한 username, password를 받게 됨
```javascript
//회원가입
passport.use('local-join', new LocalStrategy({
    usernameField : 'email',
    passwordField : 'password',
    passReqToCallback : true
}, function(req, email, password, done){
    //console.log('local-join callback called')
    var query = connection.query('select * from user where email=?', [email], function(err, rows){
        if(err) return done(err);
        if(rows.length) {
            console.log('existed user');
            return done(null, false, {message : 'your email is already used'})
        } else {
            var sql = {email : email, pw : password};
            var query = connection.query('insert into user set ?', sql, function(err, rows){
                if(err) throw err;
                return done(null, {'email' : email})
            })
        }
    })
}
));

//로그인
passport.use('local-login', new LocalStrategy({
    usernameField : 'email',
    passwordField : 'password',
    passReqToCallback : true
}, function(req, email, password, done){
    var query = connection.query('select * from user where email=?', [email], function(err, rows){
        if(err) return done(err);
        if(rows.length) {
            return done(null, {'email':email})
        } else {
            return done(null, false, {'message' : 'your login info is not found >.<'})
        }
    })
}
));
```
    - done은 인증에 대한 성공과 실패를 전송해주는 콜백함수
    - 성공이면 done 두번째 인자에 세션에 저장할 값을 넘기고 실패면 null을 넘겨 인증실패를 유도

### 세션 설정
```javascript
passport.serializeUser(function(user, done){
    console.log('passport session save : ', user.email)
    done(null, user.email);
})

passport.deserializeUser(function(email, done){
    console.log('passport session get email : ', email)
    done(null, email);
})
```
    - 인증이 정상적으로 종료되면 serializeUser를 호출해 세션에 저장
    - deserializeUser은 인증 검사를 할 시 세션값을 참조하게 되면 호출

### 접근권한 라우팅 구성
```javascript
router.post('/adminpage', isAuthenticated, function(req, res){
    res.send('admin page')
})
```
    - isAuthenticated 호출시 중간에 인터셉터를 하여 권한 체크 
    - 정상적인 인증상태면 아래 로직 수행 / 비정상 상태일 경우 로그인 페이지로 이동

### 인증검사
```javascript
var isAuthenticated = funcion(req, res, next){
    if(req.isAuthenticated()) return next();
    res.send('not login');
}
```
    - request를 받아서 요청된 인증사항이 정상적인지 검사
    - 정상적이면 next를 콜백해 원래 기능 수행 / 비정상이면 재로그인 유도

###### references https://m.blog.naver.com/PostView.nhn?blogId=scw0531&logNo=221175584994&proxyReferer=https%3A%2F%2Fwww.google.com%2F
