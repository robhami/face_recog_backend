Create smart-brain-api folder and run npm init from terminal to add package.json. 

Then install: 
```
npm install body-parser
npm install nodemon --save-dev
npm install express
```

Create file: server.js

Add to scripts in package.json: 

```
"scripts": {
    "start": "nodemon server.js"
  },
 ``` 
  
This will autostart server when run npm start. 

Add following code to server.js: 

```
const express = require('express');

const app = express();

app.get('/' ,(req, res)=>{
	res.send('this is working');

})



app.listen(3000, ()=>{
	console.log('app is running on port 3000');
})
```

Then run NPM start and it will return 'app is running on port 3000'.

Then do GET request by entering localhost:3000 into Postman. 

Plan out structure

res= this is working
/signin -->POST = success/fail
/register -->POST = user
/profile/;userid -->GET = user
/image --> PUT --> user
