A load of stuff needs to be added: 

In server.js:

in the signin: After the if statement:
```
res.json(database.users[0]); instead of res.json("success"). 
```

In Signin.js:

To check the response above, we check the users's id to . update the Rank.
```
     .then(response => response.json())
      .then(user => {
        if(user.id){
          this.props.loadUser(user);
          this.props.onRouteChange('home');
        }
      })
```
Instead of check if it was Successful.

In Rank.js

I pass two props to update the Rank:
```
const Rank = ({name, entries}) => { 
```
and display them in the <div>:
```
{`${name} , your current rank is...`}
      <div className='white f1 '>
        {entries}
      </div>
```
In App.js:
```
             <Signin loadUser={this.loadUser} onRouteChange={this.onRouteChange} /> 
```
To load the User's id (like I mentioned above)
```
              <Rank name={this.state.user.name} entries={this.state.user.entries}/> 
```
To update the Rank with the states of the user (loaded above)

Finally, in App.js, make sure your state looks like below and you add the loadUser method:

RH comment: That state stuff all seemed to be in order.

In server.js change image request to a put because updating user info.

```
app.put('/image',(req,res)=>{
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

Add the following to App.js:

```
onButtonSubmit = () => {
    this.setState({imageUrl: this.state.input});
   
    app.models.predict(
      Clarifai.FACE_DETECT_MODEL,
      this.state.input)
      ****
    .then(response =>{
      if(response) {
        fetch('http://localhost:3000/image', {
          method: 'put',
          headers: {'Content-type': 'application/json'},
          body: JSON.stringify({
          id: this.state.user.id
          })
        })
        .then(response => response.json())
        .then(count=>{
          this.setState(Object.assign(this.state.user,{
            entries: count }))
        })
      }
    ****
    this.displayFaceBox(this.calculateFaceLocation(response))
    })
    .catch(err => console.log(err));
  }
  
```
