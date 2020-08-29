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


