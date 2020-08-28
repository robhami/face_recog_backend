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
Then write code in server.js:
```javascript
app.post('/signin',(req,res)=>{
	if(req.body.email === database.users[0].email) &&
		req.body.password === database.users[0].password)
	res.json('success');
	} else {
		res.status(400).json('error logging in');
	}
})
```
This will check Postman added user email and password against database. However database data needs to be parsed into JSON first:
```



