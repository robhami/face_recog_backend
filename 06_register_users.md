
In register.js change to class and add functions from signIn plus a new one for name. Then change the properties to email, name and password:

```
import React, {Component} from 'react';


class Register extends Component {
	constructor (props) {
		super(props);
		this.state={
			email:'',
			password:'',
			name:''
		}
	}

	onNameChange =(event)=> {
		this.setState({name: event.target.value})	
	}
	onEmailChange =(event)=> {
		this.setState({email: event.target.value})	
	}

	onPasswordChange =(event)=> {
		this.setState({password: event.target.value})	
	}

	render(){
		return (
			<article className="br3 ba b--black-10 mv4 w-100 w-50-m w-25-l mw6 shadow-5 center">
			<main className="pa4 black-80">
			<div className="measure">
				<fieldset id="sign_up" className="ba b--transparent ph0 mh0">
					<legend className="f1 fw6 ph0 mh0">Register</legend>
					<div className="mt3">
						<label className="db fw6 lh-copy f6" htmlFor="name">Name</label>
						<input className="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="name" name="name"  id="name"/>
					</div>
					<div className="mt3">
						<label className="db fw6 lh-copy f6" htmlFor="email-address">Email</label>
						<input className="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="email" name="email-address"  id="email-address"/>
					</div>
					<div className="mv3">
						<label className="db fw6 lh-copy f6" htmlFor="password">Password</label>
						<input className="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="password" name="password"  id="password"/>
					</div>
				</fieldset>
				<div className="">
					<input 
					onClick={() => onRouteChange('home')}
					className="b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6 dib" 
					type="submit" 
					value="Register"/>
				</div>
			
			</div>
			</main>
			</article>
			);

	}
}

export default Register;
```
Create user profile and associated function in App.js: 
```
class App extends Component {
  constructor() {
    super();
    this.state = {
      input: '',
      imageUrl: '',
      box: {},
      route: 'signin',
      isSignedIn: false,
      user:{
        id: '', 
        name:'', 
        email:'', 
        entries:0, 
        joined: ''
      }
    }
  };

loadUser = (data) => {
  this.setState({user:{
     id: data.id, 
      name:data.name, 
      email:data.email, 
      entries:data.entries, 
      joined: data.joined
  }})
}
```
Add onChange call function (e.g. onNameChange) on all the other inputs. Also add onSubmitSignIn function and call that from submit button:
```
onSubmitSignIn = () => {
		fetch('http://localhost:3000/signin', {
			method: 'post',
			headers: {'Content-type': 'application/json'},
			body: JSON.stringify({
				email: this.state.email,
				password: this.state.password,
				name: this.state.name
			})
		})
		.then(response => response.json())
		.then(user => {
			if (user) {
				this.props.loadUser(user)
				this.props.onRouteChange('home');
			}
		})
		
	}

	render(){
		
		return (
			<article className="br3 ba b--black-10 mv4 w-100 w-50-m w-25-l mw6 shadow-5 center">
			<main className="pa4 black-80">
			<div className="measure">
				<fieldset id="sign_up" className="ba b--transparent ph0 mh0">
					<legend className="f1 fw6 ph0 mh0">Register</legend>
					<div className="mt3">
						<label className="db fw6 lh-copy f6" htmlFor="name">Name</label>
						<input className="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" 
						type="name" 
						name="name"  
						id="name"
						onChange={this.onNameChange}
						/>
					</div>
					<div className="mt3">
						<label className="db fw6 lh-copy f6" htmlFor="email-address">Email</label>
						<input className="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" 
						type="email" 
						name="email-address"  
						id="email-address"
						onChange={this.onEmailChange}/>
					</div>
					<div className="mv3">
						<label className="db fw6 lh-copy f6" htmlFor="password">Password</label>
						<input className="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" 
						type="password" 
						name="password"  
						id="password"
						onChange={this.onPasswordChange}
						/>
					</div>
				</fieldset>
				<div className="">
					<input 
					onClick={this.onSubmitSignIn}
					className="b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6 dib" 
					type="submit" 
					value="Register"/>
				</div>
			
			</div>
			</main>
			</article>
			);

	}
}

export default Register;
```
Add text in *** tags below to App.js:
```
 <FaceRecognition box={box} imageUrl={imageUrl}/>
           </div>
           : (
              route === 'signin' 
              ? <SignIn onRouteChange={this.onRouteChange}/>
              : <Register *** loadUser={this.loadUser} *** onRouteChange={this.onRouteChange}/>
            )       
        }
      </div>
      
```
Need to remove password from server.js:
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
		entries:0, 
		joined: new Date()
	})
	res.json(database.users[database.users.length-1]);
})
