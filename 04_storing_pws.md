### Note about bcrypt-nodejs ###
In NPM there is pacakage called bcrypt-nodejs. It is now deprecated. They recommend you now use: 

bcrypt or bcrypt.js. One is in C++ and the other is in js. 

For this lecture we will use deprecated version because it works on all systems OS's. Later can change to new versions.

### Background ###
We should not store passwords as plain text. Store them in hashes. 

Need to pass in over HTTPs. 

### Install bcrypt-nodejs ###

npm install bcrypt-nodejs

copy asynchronous code from NPM bcrypt-node js webpage. Then add to code at bottom but above app is running log comment it out for now. 
```
bcrypt.hash("bacon", null, null, function(err, hash) {
    // Store hash in your password DB.
});

// Load hash from your password DB.
bcrypt.compare("bacon", hash, function(err, res) {
    // res == true
});
bcrypt.compare("veggies", hash, function(err, res) {
    // res = false
});
```
Add to top: 
```
const bcrypt = require('bcrypt-nodejs');
```
Then add hash function from import bcrypt code to register but change password "bacon" to password: 

```
app.post('/register',(req,res)=>{
	const {email, name, password} =req.body;
	bcrypt.hash(password, null, null, function(err, hash) {
    	console.log(hash);
	});

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
This returns the following in terminal: 

$2a$10$iIcwH6pSl5yiKTjrJQqGYuhdDQCpBza53axVhs6ZjYkyk0V8jCmAe

If you do it again the hash changes: 
$2a$10$7vGlipFY8OlzeNVRfu4BL.y23Q61Hpj4AGiV35gL1JraJPB7pk5AS

Again: 
$2a$10$TKttvj3RGX/.0Gz2qzyYjOIOS9bmEm.4mZTs1xfJW7IbaacWlJw3m

add following code to signin section using hash values:
```
app.post('/signin',(req,res)=>{
	bcrypt.compare("apples", '$2a$10$TKttvj3RGX/.0Gz2qzyYjOIOS9bmEm.4mZTs1xfJW7IbaacWlJw3m', function(err, res) {
    	console.log('first guess', res)
	});
	bcrypt.compare("bacon", '$2a$10$TKttvj3RGX/.0Gz2qzyYjOIOS9bmEm.4mZTs1xfJW7IbaacWlJw3m', function(err, res) {
    	console.log('second guess', res)
	});

	if(req.body.email === database.users[0].email &&
	req.body.password === database.users[0].password) {
	res.json('success');
	} else {
		res.status(400).json('error logging in');
	}
})

```
This return the following in terminal: 

first guess true
second guess false
