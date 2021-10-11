# NodeJs

### creating a simple server 

```
var http = require("http");
http.createServer(function(req,res){
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end('HEllo bitch!');
}).listen(8080);
```
#### req argument of createServer
reprsents the request from the client as an object 
` http.IncomingMessage` object 
This object has a property called "url" which holds the part of the url that comes after the domain name
```
var http = require("http");

http.createServer(function(req,res){
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(req.url+"<br>");
    res.end('HEllo bitch!');
}).listen(8080);
```

[Rest of http](https://www.w3schools.com/nodejs/nodejs_http.asp)

###  creating your own module 

```
exports.blabla = function(){
    return fuckfuck;
}
```

### include your own module 

let's say you saved that file as bitch.js then 

```
var mod = require("./bitch.js");
```

we are using ./ here only when the module is located in the same folder as the nodejs




