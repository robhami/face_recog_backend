
### Profile Id ###
If use :id syntax we can enter in our browser anything and we'll be able to grab this id thru the re.params property rights. 

```
app.get('/profile/:id', (req, res) => {
	const {id} = req.params;
	database.users.forEach(user => {
		if(user.id === id) {
			res.json(user);
		} else {
			res.status(404).json('no such user')
		}
	})
})
```

This returns user details for: 

localhost:3000/profile/123

but not for

localhost:3000/profile/124

because it is returning user not found for first one then it can't send another response. 
So need to change code to remove 'no such user' (and also it will be more efficient if it stop and return a match): 

```
app.get('/profile/:id', (req, res) => {
	const {id} = req.params;
	let found=false;
	database.users.forEach(user => {
		if(user.id === id) {
			found=true;
			return res.json(user);
		} 
	})
	if(!found){
		res.status(404).json('no such user');
	}
})
```

### Image ###
Add following code to count entries: 
```
app.post('/image',(req,res)=>{
	const {id} = req.body;
	let found=false;
	database.users.forEach(user => {
		if(user.id === id) {
			found=true;
			user.entries++;
			return res.json(user.entries);
		} 
	})
	if(!found){
		res.status(404).json('no such user');
	}
})
```

Then if do a POST: localhost:3000/image on Postman with the following in body (with Raw JSON selected): 
```
{
	"id":"124"
}
```
This will return 1 then 2 then 3 counting up with each post. 

