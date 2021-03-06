Do testing in Postman before adding to front-end. 

Add following code to server.js: 

```javascript
app.post('/signin',(req,res)=>{
	res.send('signin')
})
```

Then test using Postman by inputting 'localhost:3000/signin'.

Change send method to JSON method as it has added feature i.e. it responds with JSON , 
```javascript
app.post('/signin',(req,res)=>{
	res.json('signin')
})
```
Then test using Postman by inputting 'localhost:3000/signin'.

### Database ###

Create variable to act as database: 

```javascript
const database ={
	users: [
	{id: '123', 
	name:'John', 
	email:'john@gmail.com', 
	password:'cookies', 
	entries:0, 
	joined: new Date()
	},
	{id: '124', 
	name:'Sally', 
	email:'sally@gmail.com', 
	password:'bannanas', 
	entries:0, 
	joined: new Date()
	}
	]

}
```
Then add data to Postman body to act as if it is receiving as if it were the fornt-end: 

```javascript
{
	"email":"john@gmail.com", 
	"password":"cookies" 
}
```
### Conditional to check entry against database ###

Then write code in server.js:
```javascript
app.post('/signin',(req,res)=>{
	if(req.body.email === database.users[0].email &&
	req.body.password === database.users[0].password) {
	res.json('success');
	} else {
		res.status(400).json('error logging in');
	}
})
```
This will check Postman added user email and password against database. However database data needs to be parsed into JSON first:
```
const express = require('express');
** const bodyParser = require('body-parser'); **

const app = express();

** app.use(bodyParser.json()); **

```
Then test in Postman and it will return 'success'. 

Could do for loop through but will be unnecessary once proper database used. 

### Register ###

Add to Postman: 

```
{
	"email":"ann@gmail.com", 
	"password":"apples",
    "name":"Ann"

}
```

Then create this code (note destrcturing used to get values. Also values that will be filled have no parenthesis and just list property that will be replace with value from Postman): 
```
app.post('/register',(req,res)=>{
	const {email, name, password} =req.body;
	database.users.push({
		id: '125', 
		name: name, 
		email: email, 
		password:password, 
		entries:0, 
		joined: new Date()
	})
	res.json(database.users[database.users.length-1]);
})

```

This will return in Postman response pane: 

```
{
    "id": "125",
    "name": "Ann",
    "email": "ann@gmail.com",
    "password": "apples",
    "entries": 0,
    "joined": "2020-08-29T06:12:40.502Z"
}
```
To get list of all users, change get request that responded with "this is working" to respond with database.users: 
```
app.get('/' ,(req, res)=>{
	res.send(database.users);

})
```
This return all users except Ann who was just added: 
```
[
    {
        "id": "123",
        "name": "John",
        "email": "john@gmail.com",
        "password": "cookies",
        "entries": 0,
        "joined": "2020-08-29T06:15:25.507Z"
    },
    {
        "id": "124",
        "name": "Sally",
        "email": "sally@gmail.com",
        "password": "bannanas",
        "entries": 0,
        "joined": "2020-08-29T06:15:25.507Z"
    }
]

```
If redo post register and get users then Anne appears. This is because changed root route to include database.users. i.e. 
```
app.get('/' ,(req, res)=>{
	res.send(database.users);

})
```


So nodemon server had to restart. Everytime we restart variable assignments get rerun so database just initiates with 2 users. So we need a database. 

