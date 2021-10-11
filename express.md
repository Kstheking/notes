# Express

- Express is a web framework 

```
const express = require('express');
const app = express();

app.get("/", function(req, res) {
 res.send('Hello world');
});
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```

- express has a router so we can store routes in a seperate files and export 
- we can parse incoming request with bodyParser 
- res can send, sendFile, render, json

## middleware 

- middleware functions are functions that have access to the request and response object. Express has built in middleware but there are also third party libraries, (kinda like angular interceptor I guess)
- path module is a module to deal with file paths `res.sendFile(path.join(__dirname,'public','index.html'))` 
- when u create a middleware it takes three stuff req res and next 

```
const logger = (req, res, next) => {
  console.log("alright cool");
  next();
}

app.use(logger);
```

## static folder 

- app.use is used to include middleware 
- Static files are files that don't change when your application is running.
- `app.use(express.static(path.join(__dirname,'public')));`
- after doing this you could just access static files like `localhost:5000/about.html`
- to send json response `res.json(somethingSomething)` somethingSomething is a js object 

- you could export module as `module.exports = members;`

## more get request 

```
app.get('/api/members/:id', (req, res) => {
  const found = members.some(member => member.id == req.params.id);
  if(found){
    res.json(members.filter((member) => member.id == req.params.id));
  // if u use === above then use parseInt on req.params.id because it is string 
  }
  else {
    res.status(400).json({msg : `member not found with id : ${req.params.id}`});
  }
});
```

## using router 

- can be used to declutter and move routes in a seperate file 
- in the file for the routes 

```
const express = require('express');
const router = express.Router();
//define route normally but instead of `app.get...` use `router.get..`
module.exports = router;
```

- to use this include the below in index.js

```
app.use('/api/members', require('./routes/api/members'));
```

- also remember since the route has hit `/api/members` the route to be hit in the above file will be left over such as `/` or `/:id`

## body parser 

- in order to read HTTP POST data , we have to use "body-parser" node module. body-parser is a piece of express middleware that reads a form's input and stores it as a javascript object accessible through req.body

```
app.use(express.json()); //handle raw json
app.use(express.urlencoded({extended: false})); //parse query string
```

- [extended](https://stackoverflow.com/questions/29960764/what-does-extended-mean-in-express-4-0) basically if extended is true you can have nested objects in query string

- express.urlencoded parses requests which have urlencoded payloads 
- res can also redirecft `res.redirect("/")`
