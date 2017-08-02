# ajax-interceptor
ajax-interceptor (replace the default ajax and handle the HTTP request/response )

**install**
```
npm install ajax-interceptor
```


```js
new AjaxInterceptor(triggers);
```

**triggers is array in below**

参数 | 说明
---|---
 keyword | keywords in request url
response | response callback function,get response header and body
request | callback for http request(post data, and response data) 
dataType | response data type you like(json(default)/text)
cancle |  do not look up below in the trigger list (default:false)


**Examples:**

```js
const AjaxInterceptor = require('ajax-interceptor');

var triggers = [];
triggers.push({
    keyword: '/jslogin?',
    'response': (json) => {
        //response json obj;
    }
});
triggers.push({
    keyword: '/login?loginicon=true',
    'dataType': 'text',
    'response': (data) => {
       //response text data
    },
});
triggers.push({ 
    keyword: '/webwxinit?',
    'response': (data) => {
       
    },
    'request': (post_data, response_data) => {
         
    },
});
new AjaxInterceptor(triggers);
```
