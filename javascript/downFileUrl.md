# URL 이용해서 파일 다운로드

## AXIOS
- axios로 요청 후 헤더 정보에서 파일명 받아옴

```javascript
const fs = require('fs');
const Path = require('path');
const Axios = require('axios');

async function downloadFile(uri, folderName) {
  //폴더 생성
  const savedir = './output/' + folderName;
    if (!fs.existsSync(savedir)) {
        fs.mkdirSync(savedir);
    }

 //axios 요청
  const response = await Axios(uri, {
    method: 'GET',
    responseType: 'stream',
  });

 //파일명 지정
  const filename = decodeURI( 
    response.headers['content-disposition'].split('filename=')[1],
  );
  const path = Path.resolve(savedir, filename);
  const writer = fs.createWriteStream(path);
  response.data.pipe(writer);

  return new Promise((resolve, reject) => {
    writer.on('finish', resolve);
    writer.on('error', reject);
  });
}


```