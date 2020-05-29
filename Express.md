
# Express JS

## Table of contents
- [Introduction to Express](#introduction-to-express)
- [npm init and Package.json](#npm-init-and-package.json)
- [Automate Node Server Restart with nodemon](#automate-node-server-restart-with-nodemon)
- [More Routing](#more-routing)
- [Template and EJS](#template-and-ejs)
- [EJS: Conditionals and Loops](#ejs-conditionals-and-loops)
- [Serving Custom Assets](#serving-custom-assets)
- [Post Requests](#post-requests)

## Introduction to Express

#### What is Express:
	As per Express, Express is:
	1. Fast, unopinionated, minimalist web framework for node.
	2. It is a lightweight framework
	
- unopinionated - It means it is `flexible`, it lets you do things the way you want.


#### HTTP response/request lifecycle:
	When we go to a url asking for a webpage, we send a HTTP request,
	that request is a particular type/verb[GET,POST,etc] and also send 
	some data along with request. The server to whome we are requesting 
	has some code to decide what to send back, it gives a response.

#### Installing express
```bash
npm install express --save
```

```javascript
// /app.js

const express = require('express');

// We execute express()
const app = express();

// ////////////////
// Routes 
// ////////////////
app.get("/", function(req, res){
	res.send("hi there!");
});

app.get("/bye", function(req, res){
	res.send("Goodbye");
});

app.get("/back", function(req, res){
	res.send("Welcome Back!!!")
})

// We have to tell express to listen, otherwise our code will have no effect
// Telling Express to listen for requests (starting server)

const port = 3000;
const host = 'localhost';
app.listen(port, host, () => {
	// Call back function to tell is the server working
	console.log(`Server has started on http://${host}:${port}/`);

});
```

## npm init and Package.json 

#### Objectives
	1. Use the '--save' flag to install packages
	2. Explain what the package.json file does
	3. Use 'npm init' to create a new package.json


JSON holds meta data for a application or a specific project. 

We can create our own json file using the 'npm init' command.

By using '--save' any package we install, it will save it to the dependencies section of the JSON file.
```bash
npm install packagename --save
```

## Automate Node Server Restart with nodemon 

 For this you have to install nodemon
 ```bash
npm i -g nodemon
```

Here
- i: short for of install
- g: it will be installed globally, all over your device

To run your app.js(application file) instead of doing "node app.js" use "nodemon app.js".
- **`NOTE`**: that while using nodemon your file name that you want to run should have same name as the you have in your JSON package.

## More Routing

#### Objective
	1. Show the '*' route matcher
	2. Write routes containing route parameters
	3. Discuss route order

#### 1) The  * route:
	It will match anything.

	This is when user goes to any other route apart from the routes 
	you have defined, it will be * route and they will get response given 
	in the * route

```javascript
// /app.js

app.get("*", function(req, res){
	res.send("You are not allowed");
});
```
- `Order of your routes matter`:
If you put * route at the beginning then the routes that you have defined will not be working and when try to access the routes that you have defined you will get response of the * route.

- Therefore put the * route at the end i.e. after all the routes.

- Route Parameters:
	- In large websites there are millions of routes but that doesn't means that every route there is defined.
	- For this we have to make pattern in our routes and those who will match pattern will be given the response and other will not.

#### This a example of the pattern
```javascript
// /app.js

// If we use the below line
// app.get("/r/subredditName");
// then it will match word to word, our search url has to be same(word to word).

// To make it into route parameter/route variable or in other frameworks 
// it is called path variable we have to put colon(:) this will tell 
// express to not to match everything character to character.

app.get("/r/:subredditName" , function(req, res) {
		res.send("Welcome to the Subreddit");
});

// This just has a pattern of /r/Anything , so you cannot use other patterns

// Another example
app.get("/r/:subredditName/comments/:id/:title", function(req, res){
		res.send("Welcome to the comment section");
});

// How to access the data, route parameters from the routes.
// For this we will use the request object, the first parameter of the callback function.
app.get("/r/:subredditName/comments/:id/:title", function(req, res){
	// This will print the entire request object
	res.send(req);
});	

// req.params <= this command will give an object which have all the parameters.

app.get("/r/:subredditName/comments/:id/:title", function(req, res){
	// This will print the entire request object
	res.send(req.params);
});	

// Example
app.get("/r/:subredditName/comments/:id/:title", function(req, res){
	var subreddit = req.params.subreddit;
	res.send("Welcome to the " + subreddit.toUpperCase() + " subreddit");
});
```
## Template and EJS

<strong>Rendering HTML and Templates</strong>

#### Objective: 
	1. Use res.resnder() to render HTML (from an EJS file)
	2. Explain what EJS is and why we use it
	3. Pass variabble to EJS templates

```javascript
// /app.js

var express = require('express');
var app = express();

app.get("/", function(req, res){
	res.send("<h1>Hello World</h1>");
});

// render method
app.get("/", function(req, res){
	res.render("filename.html")
});

// But there is an issue when we use express we don't write plain HTML 
// file, there is a way for that and it requires setup for dynamic HTML files.

// Always use a views directory, that directory express is going to look for. 
// Whenever we render a file express is first going to look at views directory

// ejs --> embedded javasrcipt

// For using the ejs files you have to first install ejs --> "npm install ejs --save"
app.get("/", function(req, res){
	res.render("home.ejs")
});
// But this just a static ejs file

// Dynamic
app.get("/:Name", function(req, res){
	var name = req.params.Name;
	res.send("Your name is " + name);
});

// But we want to send dynamic html ejs file
// ejs let's do that also, it lets us to add loops, variables, etc. 
// It lets us embedded JavaScript code inside HTML.

app.get("/:Name", function(req, res){
	var name = req.params.Name;
	res.render("home.ejs", {name: name});
	// This second parameter is for to use name varaible in home.ejs file 
});

const port = 3000;
const host = 'localhost';
app.listen(port, host, () => {
	// Call back function to tell is the server working
	console.log(`Server has started on http://${host}:${port}/`);

});
```

```html
<!-- In /views/home.ejs -->

<h1>Form home.ejs file</h1>
<h1>Your name is <%= name.toUpperCase() %></h1>

<!-- There are rules of what JS should come here and what shouldn't -->
```

## EJS: Conditionals and Loops

#### Objective: 
	EJS Control Flow
		1. Show examples of control flow in EJS templates
		2. Write if statements in an EJS File
		3. Write loops in a EJS file

```javascript
// /app.js

var express = require('express');
var app = express();


app.get("/:Name", function(req, res){
	var name = req.params.Name;
	res.render("home.ejs", {name: name});
});

const port = 3000;
const host = 'localhost';
app.listen(port, host, () => {
	// Call back function to tell is the server working
	console.log(`Server has started on http://${host}:${port}/`);
});
```

```html
<!-- In /views/home.ejs -->

<!-- There are different types of ejs tags
	- <%= %> <-- this is used when we want something to display in the html page
	- <% %> <-- this is used when we don't want to display something
-->

<h1>Your name is <%= name %></h1>
<p>
	<% for(var i=0; i<10; i++){ %>
		<em><%= name %></em>
	<% } %>
	
	<% if (name === "King") { %>
		<strong>Your name is King</strong>
	<% } else { %>
		<strong>Who are you?</strong>
	<% } %>
</p>
```

## Serving Custom Assets

#### Styles And Partials
	1. Show how to properly inculde public assets
	2. Properly configure our app to use EJS
	3. Use partials to fry up our code!

- To include styles
- One Bad way is to go in each templates and put the following code there
```css
<style>
	body {
		color: purple;
	}
</style>
/* This is a bad practice */
```
- Better way is to `make a css file in public directory (where we put our css and javascript files)` and link them to templates where we want them using link attribute of html.

- While including header and footer
	- <%- include('partials/header') %> => this will include the header 
	This will give an error because it doesn't know where is our header.
	
	- <%- include('partials/footer') %> => this will include the footer 

This will not include the style that is linked in the header unless we make the express to not to look at app.css in partials but to look at root directory (views) for app.css.

For that we have to put => /app.css => this tells to look at root directory for app.css

```javascript
// /app.js

var express = require("express");
var app = express();

// To tell express to serve the content of express directory.
app.use(express.static("public"));
// Without this we have to write the entire path of files in links for javascript and css files.

// We are typing ejs as extension again and again, to make it shorter
app.set("view engine", "ejs");
// This also tell express that we are using ejs as our template engine

app.get('/', (_req, res) => {
	res.render('home');
});

app.get('/fallinlovewith/:name/', (req, res) => {
	var  name  =  req.params['name'];
	const  context  = {
		'name': name,
	}
	res.render('love', context);
});

app.get('/posts/', (_req, res) => {
	var  posts  = [
		{ 'title': 'Post 1', author: 'James' },
		{ 'title': 'Artice 69', author: 'Ethan' },
		{ 'title': 'Past', author: 'Tony' },
	]
	const  context  = {
		'posts': posts,
	}
	res.render('posts', context);
});

app.get("/post", (req, res) => {
	var posts = [
		{title: "Post 1", author: "James"},
		{title: "Article", author: "Ethan"},
		{title: "Note", author: "Tony"}
	];
	res.render("posts", {posts: posts});
});

const port = 3000;
const host = 'localhost';
app.listen(port, host, () => {
	// Call back function to tell is the server working
	console.log(`Server has started on http://${host}:${port}/`);
});
```

```css
/* In /public/app.css */

body {
	background: yellow;
	color: purple;
}
```

```html
<!-- In /views/partials/header.ejs -->

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>App</title>
	
	<!-- Including css file -->
	<link href="/app.css" rel="stylesheet" type="text/css" />
	<!-- 
		We need not have to write public/app.css, we will tell express
		that our stylesheets and javascripts are inside the public 
		directory.
		
		This is done by including a line a app.js:
			app.use(express.static("public"));
	-->
	  
  </head>
  <body>
```

```html
<!-- In /views/partials/footer.ejs -->
 		<p>Trademark</p>
	</body>
</html>
```
```html
<!-- In /views/home.ejs -->

<%- include('partials/header') %>

<h1>Home Page</h1>

<p>Welcome</p>

<%- include('partials/footer') %>
```

```html
<!-- In /views/love.ejs -->

<%-  include("partials/header") %>

<h1>You fell in love with <%=  name.toUpperCase() %> </h1>

<%  if (name.toLowerCase() ===  'Ax73K') { %>
	<p>I Love You Too Darling</p>
<% } else { %>
	<p>Please Leave</p>
<% } %>

<%-  include("partials/footer") %>
```

```html
<!-- In /views/posts.ejs -->

<%-  include("partials/header") %>
 
<h1>Posts Page</h1>

<%  posts.forEach((post) => { %>
	<li>Author of <%=  post['title'] %> is <%=  post['author'] %> </li>
<% }); %>

<%-  include("partials/footer") %>
```

## Post Requests
	1. Write a post routes, and test them with postman (an application)
	2. Use a form to send a post request
	3. Use body parser to get form data 

- console.log(req.body);
- `req.body is object that will contain all of the data from request body`.
- But this will not work, it will give undefined
- Because `express doesn't create req.body for us`, we need to explicitly tell it to take the request body and turn it to javascript object for us to use called req.body. For this we will use a package called `body-parser`.

- The `'name' attribute in html is very important to add data to req.body`.

```javascript
// /app.js

const express = require("express");
const app = express();
const bodyParser = require("body-parser");
const port = '3000';
const host = 'localhost';

// Telling express to use body-parser
app.use(bodyParser.urlencoded({extended: true}));

app.set("view engine", "ejs");

// For scoping issue, it will be visible to all routes from here
var friends = ["Tony", "Steve", "Thor", "Bruce", "Natasha", "Barton"];

app.get("/", function(req, res){
	res.render("home");
});

// We use post route when we are adding data to something, we are sending
app.post("/addfriend", function(req, res){
	var newFriend = req.body.newfriend;
	// This newfriend comes from the friends.ejs file where the input 
	// has name newfriend.
	
	// Pushing this new friend to our array
	friends.push(newFriend);
	res.redirect("/friends");
});

app.get("/friends", function(req, res){
	res.render("friends", {friends: friends});
});

app.listen(port, host, () => {
	console.log(`Server has started on http://${host}:${port}/`);
});
```

```html
<!-- In /views/friends.ejs -->

<h1>Friends List Goes Here</h1>

<!-- Displaying friends in a list -->
<% friends.forEach(function(friend){ %>
	<li> <%= friend %> </li>
<% }); %>

<form action="/addfriend" method="POST">
	<input type="text" name="newfriend" placeholder="name">
	<button>You made a friend</button>
</form>
```

```html
<!-- In /views/home.ejs -->

<%- include('partials/header') %>

<h1>Home Page</h1>

<p>Welcome</p>

<%- include('partials/footer') %>
```

<hr>

### Done!!!
