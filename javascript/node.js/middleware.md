# 미들웨어

## 미들웨어란?
- 양쪽을 연결하여 데이터를 주고받을 수 있도록 중간에서 매개 역할을 하는 함수.
- 미들웨서 함수 생명주기 : request - response 응답을 주기로 종료.
- 먼저 로드되는 미들웨어 함수가 먼저 실행.

## 미들웨어 함수 유형 
### 어플리케이션
- 어플리케이션 전체 영역에서 처리 가능
- 앱에 대한 request가 발생할 때마다 실행되는 미들웨어

### 라우터
- 특정 지정 라우트가 실행되었을 때마다 실행되는 미들웨어
```javascript
var app = express();
var router = express.Router();
 
router.get('/', function (req, res, next) {
 
});

```
    - get 라우트 함수의 두번째 인자 function이 미들웨어 함수

### 에러 핸들링
- 에러를 핸들링할 때 사용하는 미들웨어
```javascript
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});
 
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};
 
  // render the error page
  res.status(err.status || 500);
  res.render('error');
});
```
    - 에러 핸들링 미들웨어는 4개의 인수(err, req, res, next)를 가징
    - 에러가 캐치되면 미들웨어 함수가 호출(next 함수를 호출하지 않으면 그 상태에 머무름)

### 써드파티 미들웨어
- 기본적으로 주어지는 미들웨어 외에 추가로 설치하여 사용해야하는 미들웨어