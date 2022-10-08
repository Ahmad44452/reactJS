# Middleware

The entire idea of middleware is to execute some code before the controller action that sends the response and after the server gets the request from the client.  Essentially it is code that executes in the middle of your request, hence the name middleware.
Let’s get forward and see the basic practical example of Middleware. For this reason, we have to setup an express server. To get started working with a Node.js project we will need to run npm init -y. This will create a basic package.json file with all of the default values filled in for you. From there the next thing to do is install Express and Nodemon by running npm i express nodemon. The nodemon is used for restarting the server after every file change. After installing, we need to add start script in package.json file, which is start: “nodemon server.js”.
Lastly, we need to create a server.js file with the following code.

```
const express = require("express")
const app = express()

app.get("/login", (req, res) => {
  res.send("Login Page")
})

app.get("/signup", (req, res) => {
  res.send("Signup Page")
})

app.listen(8080, () => console.log("Server Started"))
```
This server.js file simply sets up a server on PORT 8080 that has two routes, a login page route and a signup page route. The last thing to do is run node server.js to start up the application and if everything worked we should see a message in the console saying Server Started. We can then open up any browser to localhost:3000/login and we should see the message login Page. If we go to localhost:3000/signup we should then see the message Signup Page.

The biggest thing is that middleware functions have access to the response (res) and request (req) variables and can modify them or use them as needed. Middleware functions also have a third parameter which is a next function. This function is important since it must be called from a middleware for the next middleware to be executed.

For the explanation of middleware, we will create a logging middleware. As discussed above, a middleware has three parameters req, res and next.

```
function logger(req, res, next) {
  console.log("Inside Middleware")
  next()
}
```
Express has a few different ways we can define middleware to be used, but for this example we will make this middleware execute before every single controller action by adding it to the application level. This can be done by using the use function on the app variable like this.

Now if we restart the server, we will notice that everything is logging correctly, and the web page is properly loading.
```
Note: If there is any statement after the next() function call. Those will be executed after executing the code
called by next() function.
```
Let’s create another middleware and use it on a single route.

```
app.get("/posts", authorizeUser, (req, res) => {
  res.send("Posts Page")
})

function authorizeUser(req, res, next) {
  if (req.query.admin === "true") {
    next()
  } else {
    res.send("ERROR: You must be an admin")
  }
}
```
In the above code, we have created a new route named posts and created a middleware named authorizeUser which is a fake admin authentication program. And we are using this middleware to the posts route. This will work like only admin can access the posts route. We can also see how we used authorizeUser middleware to only posts route.
Now if we go to the posts page we should see an error, but if we go to the any other defined route this message will not show up. If we instead go to http://localhost:3000/pages?admin=true we will be sent the normal posts page since we set the query parameter of admin to true.
We can also pass data from middleware to actual request route as shown in the following code.

```
app.get("/posts", authorizeUser, (req, res) => {
  console.log(req.admin)
  res.send("Posts Page")
})

function authorizeUser(req, res, next) {
  if (req.query.admin === "true") {
    req.admin = true
    next()
  } else {
    res.send("ERROR: You must be an admin")
  }
}
```
As we can see we added value of admin in req object in the middleware and further we used that admin value to console log in the requested route.
This is all about Middleware in express. We can further study middlewares from the official docs of ExpressJS.
