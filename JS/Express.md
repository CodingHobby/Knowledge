# Express

Express.js is one of the best backend frameworks for Node.js. It's extremely easy to use, it has many great plugins, and it is, in my opinion, the best framework for RESTful APIs.


## RESTful APIs
Express APIs work by listening for requests on different URIs (called routes), and acting according to those requests.

First of all, what is a REST API?

The REST in RESTful stands for Representational State Transfer, which essentially refers to a style of web architecture that has many underlying characteristics and governs the behavior of clients and servers, and it's based on HTTP requests being sent between client and server.

Let's take a look at some practical stuff.

## Getting Express into your project
Express can be installed through `npm` or `yarn`, and later be required in your file, like such:

```javascript
const express = require('express'),
	app = express()
```

After we create an express instance, we can start listening for requests, by using the `app.listen()` function and passing it a port number (I usually use 3000, but you can use whatever you want)

## What is middleware?
Middleware is basically functions that requests go through once they reach the express server. One of the most used ones is `bodyParser`, which is needed in order to parse a request body, making it indispensable if you want to work with forms. Another really cool one is `expressValidator`, which is the easiest way to validate a form in Node.js.

But how do you actually use middleware? It's pretty straight forward: all you have to do is require it, and then pass it as a parameter to `app.use()`:

```javascript
const express = require('express'),
	bodyParser = require('body-parser'),
	app = express()

app.use(bodyParser.urlEncoded({extended: true}))
app.use(bodyParser.json())

app.listen(3000)
```
Note that we actually had to use two different `app.use()` statements: one to be able to read data from forms, and one to receive that data in the JSON format.

## Routes
As we said Express listens for requests on different URIs, and acts according to the request data (the request type, its body...).

There are two ways to write Routes: one of them is to write them directly into the `app` variable, while the other is to create an Express Router, and then use it as middleware for the app. To write them, you'll want to call either one of those variable's function to handle the desired request method:

```javascript
const express = require('express'),
	bodyParser = require('body-parser'),
	app = express()

app.use(bodyParser.urlEncoded({extended: true}))
app.use(bodyParser.json())

app.get('/', function(req, res) {
	return res.send({uri: req.url})
})

app.post('/register', function(req, res) {
	return res.send({
		username: req.body.username,
		password: req.body.password
	})
})

app.listen(3000)
```

Here are two examples of a GET and a POST request: as you can see, to handle them Express needs a function with two parameters: a request and a response.

## Static Content
This is cool and all, but what if we want to reply to a request with an actual web page? There are two main ways to do that: either with static content or with views.

Let's talk about static content for a moment, shall we?

How would we go about serving up HTML files? Well, Express has a built in tool to do just that, and it's called `express.static`.

```javascript
const path = require('path')
app.use(express.static(path.join(__dirname, 'public')))
```

It's just a piece of middleware, and it takes the absolute path to the folder with all the static content as an argument. At this point, in our code, we can just say `res.send()` and pass the name of a file as an argument.

## Views
Views in Express are just the dynamic way of rendering: they use a templating language to create HTML content based on data that is passed to them from the backend layer. To use views, first of all you'll want to have a `views` folder, and inside it a `layouts` folder:

```
.
└── views
    ├── home.handlebars
    └── layouts
        └── main.handlebars
```

At this point you'll want to install the `express-handlebars` plugin through `npm`, to then require it in your code:

```javascript
const express = require('express'),
	exphbs = require('express-handlebars'),
	app = express()

app.engine('handlebars', exphbs({defaultLayout: 'main'}))
app.set('view engine', 'handlebars')

app.get('/', function(req, res) {
	return res.render('home', {msg: 'Hello, world!'})
})

app.listen(3000)
```

First of all, we sets the app's view engine to handlebars (lines 5-6), and we pass a configuration JSON object, which contains a `defaultLayout` field, which is the layout Express should normally render (it has to be the name of a file in the `views/layouts` folder). In the layout file, we want to have something that looks like this:

```html
<html>

<head>
	<title>Express Demo</title>
</head>

<body>
  {{{body}}}
</body>

</html>
```

Everything looks fine, but what is that `{{{body}}}` thingy?

Well, since we want to render different views depending on the request URI, we want to be able to render different views in the same layout, and that is what `{{{body}}}` is: it renders the view that you request from the server.

In the `views/home.handlebars` file, we want to have this code:

```html
<div class="home">
  <h1>{{msg}}</h1>
</div>
```

We can access variables that we pass in from the server by using double curly braces, but that's just handlebars stuff, thanks to which you can also do much more complicated things, such as if statements, for and while loops etc...

In conclusion Express is pretty cool and easy to use, and it's great for server-side rendering as well as RESTful APIs.
