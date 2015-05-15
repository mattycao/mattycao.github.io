---
layout:     post
title:      How to write Middleware for Express.js App
date:       2015-05-14 23:27:00
summary:    The details and some pitfalls of writeMiddleware for express.js application
categories: Node
---

## How to write Middleware for Express.js App
[The Original Source](https://stormpath.com/blog/how-to-write-middleware-for-express-apps/)

Express.js is a lightweight HTTP framework for node.js.

The structure of ExpressJS is this: everything is 'Middleware'.

```javascript
app.use(bodyParser());
app.use(cookieParser());
```

So what middleware is?
### What is Middleware?
Middleware is a function that receives the request and response objects of an HTTP request/response cycle. It may modify (transform) these objects before passing them to the next middleware function in the chain. It may decide to write to the response; it may also end the response without continuing the chain.

A simple middleware function:

```javascript
function logger(req,res,next){
  console.log(new Date(), req.method, req.url);
  next();
}
```
This is middleware at its simplest: a function with a signature of `(req, res, next)`. In this particular example, a simple logger prints some information about these requests to the server console, and then continues the chain by calling `next()`.

The job of Express is to manage your chain of middleware functions. All middleware should achieve three things:

* It should be a function that does something awesome
* It’s well-documented
* It can be easily mixed into your existing Express application

### The “Hello, World!” of Middleware
As you write your own middleware you will run into some pitfalls, but fear not! I will cover them in this article, so you know if you’ve fallen into one. :)

In our example application, we want two simple things to happen:

* Say “hello” and “goodbye” on all the responses.
* Log all the requests.

Let’s get started! Here is a basic shell of an Express application:

```js
var express = require('express');
var app = express();
var server = app.listen(3000);
```

This Express application doesn’t do anything by itself; you need to add some middleware! We’ll add our logger (the one you saw in the introduction):

```js
app.use(logger);
```
Good to go, right? When we run our server and make a request of it (using Curl in the terminal), we should see a log statement printed in the server console. But if you try, you’ll see this from your request:

```bash
$ curl http://localhost:3000/
Cannot GET /
$
```
And this from your server:

```bash
Mon Mar 23 2015 11:05:04 GMT-0700 (PDT) 'GET' '/'
```
**We saw the server logging, but Curl gets a ‘Not Found’ error. What happened?**

#### Pitfall #1 – “Not Found” means “Nothing else to do”

While we did the good thing of calling `next()`, no other middleware has been registered and there is nothing else for Express to do!

Because we have not ended the response, and there are no other middleware functions to run, Express kicks in with a default “404 Not Found” response.

***The Solution***:
end your responses via `res.end()`. We’ll cover that in a later section.

##### Saying Hello

We’re going to write a new middleware function, named `hello`, and add this to our app. First, the function:

```js
function hello(req,res,next){
  res.write('Hello \n');
  next();
}
```
Then add it to your middleware chain:

```js
app.use(logger);
app.get('/hello',hello);
```
Notice the difference between `use` and `get`? These mean different things in Express.js:

* `use` means “Run this on ALL requests”
* `get` means “Run this on a GET request, for the given URL”

So what happens when we make a request for `/hello`? Here’s what Curl would do:

```bash
$ curl http://localhost:3000/hello
Hello
```
And what the server logs:

```bash
Mon Mar 23 2015 11:23:59 GMT-0700 (PDT) 'GET' '/hello'
```
Looks okay, right? But if you actually try this, you’ll notice that your Curl command never exits. Why? Read on…

#### Pitfall #2 – Not Ending The Response

Our `hello` middleware is doing the right thing and writing “Hello” to the response, but it never ends the response. As such, Express will think that someone else is going to do that, and will sit around waiting.

***The solution***:
You need to end the response by calling `res.end()`.

We wanted to say “Bye” as well, so let’s create some middleware for that. In that middleware we will end the response. Here’s what the `bye` middleware looks like:

```js
function bye(req,res,next){
  res.write('Bye \n');
  res.end();
}
```
And now we’ll add it to the chain:

```js
app.use(logger);
app.get('/hello',hello,bye);
```
Now everything will work the way we want! Here is what Curl gives us:

```bash
$ curl http://localhost:3000/hello
Hello
Bye
$
```
And the server will log our requests as well:

```bash
Mon Mar 23 2015 11:23:59 GMT-0700 (PDT) 'GET' '/hello'
```
### Middleware: Mix and Match
When you create simple middleware functions with single concerns, you can build up an application from these smaller pieces.

Here’s a hypothetical situation: let’s say we’ve setup an easter-egg route on our server, named `/wasssaaa` (rather than `hello`). Of course, we want to know how many people hit this route, because that’s really quite interesting. But we don’t want to know if someone is hitting the `hello` route, because that’s not very interesting.

Besides, our marketing team can tell us that information from their analytics system (but they don’t know about our easter egg! Ha!)

We would re-write our midddleware wiring to look like this:

```js
app.get('/hello',hello,bye);
app.get('/wasssaaa',logger,hello,bye);
```
This removed the global `app.use(logger)` statement, and added it to just the `/wasssaaa` route. Brilliant!

### Express.js Routers
Express.js has a feature called `Routers` – mini Express applications that nest within each other. This pattern is a great way of breaking up the major components of your application.

A typical situation is this: you have a single Express server, but it does two things:

* It serves a few basic pages, such as a home page and user registration system
* It serves an API that your customers use to access their information, perhaps from mobile applications, an Angular application, or some other service

The code and dependencies for the web pages are drastically different from the API service, and you typically divide up that code. You can also divide them into separate Express routers!

### Hello World, with Routers
Following the situation above, let’s build a simple server that says Hello! to everyone on our website AND our API, but only logs requests for the API service. That simple app might look like this:

```js
var app = express();
var apiRouter = express.Router();
apiRouter.use(logger);
app.use(hello,bye);
app.use('/api',apiRouter);
```
We’ve separated our API service by defining it as an Express router, and then attaching that router to the `/api` URL of our main Express application.

What happens when we run this? Here’s what Curl reports:

```bash
$ curl http://localhost:3000/
Hello
Bye

$ curl http://localhost:3000/api/hello
Hello
Bye
$
```
Looking good, right? But what does the server have to say?

```bash
...
```
***Hmm.. it doesn’t say anything! What happened??***

#### Pitfall #3: Ordering of Middleware is Important

Express.js views middleware as a chain, and it just keeps going down the chain until a response is ended or it decides there is nothing left to do. For this reason, the order in which you register the middleware is very important.

In the last example we registered the `bye` middleware before we attached the `apiRouter`. Because of this, the `bye` middleware will be invoked first. Our `bye` middleware ends the response (without calling `next`), so our chain ends and the router is never reached!

***The Solution***:
Re-order your middleware. In this situation, we simply need to register the `apiRouter` first:

```js
app.use('/api',apiRouter);
app.use(hello,bye)
```
With this, we now get what we expect. Curl reports the same as before:

```bash
$ curl http://localhost:3000/
Hello
Bye
$ curl http://localhost:3000/api/hello
Hello
Bye
$
```
But now our server logs the API request:

```bash
Mon Mar 23 2015 14:48:59 GMT-0700 (PDT) 'GET' '/hello'
```
But notice something curious here: even though we have requested `/api/hello`, the logger reports `/hello`. Hmm…

#### Pitfall #4: URLs Are Relative to a Router

A router doesn’t really know that it’s being attached to another application. If you really need to know the exact URL, you should use `req.originalUrl`. Or if you’re curious about where this route has been attached, use `req.baseUrl` (which would be `/api` in this example).

### Debugging Middleware

In this article I’ve shown you some of the common pitfalls with Express.js middleware. This certainly will help you as you write your own middleware, but what about all that third party middleware that you put into your Express application?

If you’re using 3rd party middleware and it’s not working as expected, you’re going to have to stop and debug.

First, look at the source code of the module. Scan it for places where it accepts `(req,res,next)` and look for places where it calls `next()` or `res.end()`. This will give you a general idea of what the flow control looks like, and might help you figure out the issue.

Otherwise, check out Node Inspector and go down the step-by-step debugging path.

### So Much Middleware, So Little Time!
I’ll wrap this article with this advice: there’s a lot of middleware out there! Start browsing NPM for great node.js middleware! If you want some inspiration, here are some we love:

* **Morgan** is a fully-featured logger for Express.js. It does much more than our trivial example!
* **Helmet** is a collection of smaller modules, culminating in a one-stop-shop for securing your web application. You should install this one right now.
