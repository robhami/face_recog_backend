Do NPM start for React app and server.js. This will tell you both are running on port 3000 because 3000 is default port for React. 
```
? Something is already running on port 3000.

Would you like to run the app on another port instead? (Y/n)
```

Can either change server port in code or just click Y. Clicking Y will ut React on port 3001. 

### Send data to React App ###
Add at top of App.js componentDidMount function: 
```

class App extends Component {
  constructor() {
    super();
    this.state = {
      input: '',
      imageUrl: '',
      box: {},
      route: 'signin',
      isSignedIn: false
    }
  };

componentDidMount () {
  fetch('http://localhost:3000/')
  .then(response => response.json())
  // .then(data => console.log(data))- can write shortened as just console.log below
  .then(console.log)
}
```
This runs but gives the following error message due to google security: 

```
Access to fetch at 'http://localhost:3000/' from origin 'http://localhost:3001' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```

To get around this need to use NPM package called cors. 

do NPM install cors

then call it in server.js code: 
```
const cors = require('cors');
```
then add : 
```
app.use(bodyParser.json());
** app.use(cors()); **
```
Run NPM start then app has no error and an array with 2 users:
```
Array(2)
0: {id: "123", name: "John", email: "john@gmail.com", entries: 0, joined: "2020-08-29T11:21:17.870Z"}
1: {id: "124", name: "Sally", email: "sally@gmail.com", entries: 0, joined: "2020-08-29T11:21:17.870Z"}
length: 2
__proto__: Array(0)
```

### Set up sign in ###

Add passwords back to users in server.js. 

Remove componentDidMount from the app cos don't need it anymore. 

Need to give signin component in Signin.js a state. 

Change signIn component to class and add render around return also destructure to add this.props because we receive props onRouteChange function. Also create onEmailchange & onPasswordChange functions to listen for changes to inputs. And create state add constructor:
** Note when adding constructor have to do the following: 
```
import React, {Component} from 'react';


class SignIn extends Component {
```
As opposed to what Andei says: 
```
import React from 'react';

class SignIn extends React.component {
```

Here is the code: 
```
class SignIn extends React.component {
	
	constructor (props) {
		super(props);
		this.state={
			signInEmail:'',
			signInPassword:''
		}
	}

	onEmailChange =(event)=> {
		this.setState({signInEmail: event.target.value})	
	}

	onPasswordChange =(event)=> {
		this.setState({signInPassword: event.target.value})	
	}

	

	render() {
		const {onRouteChange} = this.props;

		return (
			<article className="br3 ba b--black-10 mv4 w-100 w-50-m w-25-l mw6 shadow-5 center">
			<main className="pa4 black-80">
			<div className="measure">
				<fieldset id="sign_up" className="ba b--transparent ph0 mh0">
					<legend className="f1 fw6 ph0 mh0">Sign In</legend>
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
					value="Sign in"/>
				</div>
				<div className="lh-copy mt3">
					<p onClick={() => onRouteChange('register')} className="f6 link dim black db pointer">Register</p>

				</div>
			</div>
			</main>
			</article>
			);
	}

}
```

Then need to add onSubmitSignIn function that will just call state for now. 

```

onPasswordChange =(event)=> {
		this.setState({signInPassword: event.target.value})	
	}

	onSubmitSignIn = () => {
		console.log(this.state);
		this.
	}
  ```
Then change onclick input sign to:
```
<input 
					onClick={this.onSubmitSignIn}
					className="b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6 dib" 
					type="submit" 
					value="Sign in"/>
 ```
 And add onRoute change to onSumbitSignIn function:
 ```
 onSubmitSignIn = () => {
		console.log(this.state);
		this.props.onRouteChange('home');
	}
  ```
  This returns: 
  
  ```
  {signInEmail: "", signInPassword: ""}
signInEmail: ""
signInPassword: ""
__proto__: Object
```

Because we haven't add the email and password functions to their respective inputs. 

```
		<div className="mt3">
						<label className="db fw6 lh-copy f6" htmlFor="email-address">Email</label>
						<input 
						className="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" 
						type="email" 
						name="email-address"  
						id="email-address"
						onChange={this.onEmailChange}
						/>
					</div>
					<div className="mv3">
						<label className="db fw6 lh-copy f6" htmlFor="password">Password</label>
						<input 
						className="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" 
						type="password" 
						name="password"  
						id="password"
						onChange={this.onPasswordChange}
						/>
					</div>
          
```
