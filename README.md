let nock = require('nock');
let axios = require('axios');

(async ()=>{
    nock('https://third/party/api')
    .post('/url')
    .reply(201, { id: '123ABC' })
  
    var options = {
      'method': 'POST',
      'url': 'https://third/party/api/url',
      'headers': {
        'X-API-KEY': 'E6xwOg2BY4RbmiJogfyegrt746r7te',
        'Content-Type': 'application/json'
      }
    };
    const result = await axios(options);
    console.log(`data :${JSON.stringify(result.data)} & status code: ${JSON.stringify(result.status)}`)
})()


{
  "name": "nock-example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.21.4",
    "express": "^4.17.1",
    "nock": "^13.1.3"
  }
}
