let nock = require('nock');
let axios = require('axios');

class Mocking {
    positiveRes = async ()=>{
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
    let result  = await axios(options);
    return {code:result.status, msg: result.data};
    }
    negativeRes = async ()=>{
        try{
        nock('https://third/party/api')
        .post('/url')
        .reply(413,{error:{ message: '123ABC',code:1 }})
      
        var options = {
          'method': 'POST',
          'url': 'https://third/party/api/url',
          'headers': {
            'X-API-KEY': 'E6xwOg2BY4RbmiJogfyegrt746r7te',
            'Content-Type': 'application/json'
          }
        };
        let result = await axios(options).catch(err=> {return err.response});
        console.log('d',result) 
        return {code:result.status, msg: result.data};   
    }
    catch(ex){
        console.log('ex',ex)
    }
    }
    errorRes = async ()=>{
        nock('https://third/party/api')
        .post('/url')
        .replyWithError('something awful happened')
      
        try {
        var options = {
          'method': 'POST',
          'url': 'https://third/party/api/url',
          'headers': {
            'X-API-KEY': 'E6xwOg2BY4RbmiJogfyegrt746r7te',
            'Content-Type': 'application/json'
          }
        };
        return await axios(options);
    }
    catch(ex)
    {   console.log('error');
        return {code:404,msg:ex};
    }
    }
}

const mocking = new Mocking()
module.exports = mocking

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

const express = require('express')
const app = express()
const mocking = require('./mock')
 

app.get('/res/positive', async (req, res) => {    
    let result = await mocking.positiveRes()
    res.status(result.code).send(result.msg)
})

app.get('/res/negative', async (req, res) => {
    let result = await mocking.negativeRes()
    res.status(result.code).send(result.msg)
})

app.get('/res/error', async (req, res) => {
    let result = await mocking.errorRes()
    console.log(result)
    res.status(result.code).send(result.msg)
})


app.listen(2003,()=>console.log('server is listing at 3002'));


--------------------------------------------------------------------------------------------------------------------------------------------------------------------

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
