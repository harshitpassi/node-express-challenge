# Backend Challenges Boilerplate - Basic Express

My notes for Node and Express:

* Node - tool to write server-side code in JS.
* Useful built-in modules:
  * HTTP: acts as server
  * File System: to read and modify files
  * Path: for working with directory and file paths
  * Assertion testing: testing code against prescribed constraints
* Express: middleware - runs between Node server and FE app. Handles app routing.
  * Routes have the syntax app.<method : get, post etc>(<path>, <handler function>)
  * Handlers take the form function( req, res ){…}
  * res.send(‘') will serve a string from the handler function
  * a file can be served using res.sendFile(path)
  * this method needs absolute file path, we can use the __dirname global variable to get it. absolutePath = __dirname+relativePath
  * Need to use middleware function express.static(path) to place static assets.  Middleware functions need to be mounted   * using app.use(path, middlewareFunction).  Path is optional, and if not specified, will be applied for all requests.
  * res.json( Object ) is used to serve JSON data. Converts valid js object into string and headers to tell the browser that you’re serving JSON.
* .env is a hidden file used to pass environment variables to the application.
  * environment variables accessible as process.env.VAR_NAME.
  * process.env is a global node object.
  * Variables should be uppercase separated by _
  * .env is a shell file, no need to wrap names and values in quotes.
  * no space around equal sign: VAR_NAME=value
  * Middleware: 3 arguments, request, response, next function in request-response cycle. Can also end the cycle by sending response if some condition is met. [More Info](http://expressjs.com/en/guide/using-middleware.html “More info”)
* Express evaluates functions/routes in the order they appear. Middleware needs to be mounted before all routes that need it.
* Middleware can be mounted at a specific path using app.METHOD(path, middlewareFunction). Can also be chained inside route definition:
```javascript
app.get('/user', function(req, res, next) {
req.user = getTheUserSync(); // Hypothetical synchronous operation
next();
}, function(req, res) {
res.send(req.user);
})
```
* This approach can be used to perform data validation. At any point, control can be sent to error handling function.
* Route parameters are found in the req.params object.
```
route_path: '/user/:userId/book/:bookId'
actual_request_URL: '/user/546/book/6754' 
req.params: {userId: '546', bookId: '6754’}
```
* Query params are found in the req.query object. Query string is delimited by ? and contains field=value couples separated by &.
```
route_path: '/library'
actual_request_URL: '/library?userId=546&bookId=6754' 
req.query: {userId: '546', bookId: '6754’}
```
* POST - data hidden in HTML request body. REST convention - POST used to create new objects in DB.
  * multipart/form-data encoding is used to upload binary files.
  * urlencoded is the default format used by HTML forms
  * body-parser - package that has series of middleware to decode data in different formats. `bodyParser.urlencoded({extended: false})` is used to decode urlencoded data.
