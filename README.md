# express-basic-auth
* Note:In the real-time projects we don't use 'base64' to encode/encrypt the username and password, which is not secured. 
* Using basic authentication approach to authenticate users. 
  - Set up an Express server to handle basic authentication
  - Use the basic access authentication approach to do basic authentication.
# Setting up the Express Server
 - Create a folder named basic-auth and then create package.json and server.js files.
 - Then go to the basic-auth folder and install all the node modules by typing the following at the prompt:

````
     npm install
````
 * Open the server.js file write the code as follows:

`````
var express = require('express');
var morgan = require('morgan');
var hostname = 'localhost';
var port = 3000;
var app = express();
app.use(morgan('dev'));
function auth (req, res, next) {
    console.log(req.headers);
    var authHeader = req.headers.authorization;
    if (!authHeader) {
        var err = new Error('You are not authenticated!');
        err.status = 401;
        next(err);
        return;
    }
    var auth = new Buffer(authHeader.split(' ')[1], 'base64').toString().split('
      :');
    var user = auth[0];
    var pass = auth[1];
    if (user == 'admin' && pass == 'password') {
        next(); // authorized
    } else {
        var err = new Error('You are not authenticated!');
        err.status = 401;
        next(err);
    }
}
app.use(auth);
app.use(express.static(__dirname + '/public'));
app.use(function(err,req,res,next) {
            res.writeHead(err.status || 500, {
            'WWW-Authenticate': 'Basic',
            'Content-Type': 'text/plain'
        });
        res.end(err.message);
});
app.listen(port, hostname, function(){
  console.log(`Server running at http://${hostname}:${port}/`);
});
`````

* Save the changes and start the server. Access the server from a browser and see the result
