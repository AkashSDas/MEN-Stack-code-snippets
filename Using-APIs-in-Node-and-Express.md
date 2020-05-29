# Using API's in Node and Express

- It is used for `connecting` `our application` with `other applications`.

- API full form is **`Application Programming Interface`**.

- API's are interface for code/computers to talk to one another
	Eg:  &nbsp; <em>Facebook has an api that can be used to use some facebook data in our application.</em>

# Using API's in Node

## Table of contents
- [Introduction to  API's](#introduction-to-apis)
- [XML and JSON](#xml-and-json)
- [API Request with Node](#api-request-with-node)
- [JSON Placeholder API](#json-placeholder-api)
- [Better Way of Concatenation](#better-way-of-concatenation)
- [Arrow Functions](#arrow-functions)
- [Using Promises](#using-promises)


## Introduction to API's
- It is used for `connecting` `our application` with `other applications`.

- API full form is **`Application Programming Interface`**.

- API's are interface for code/computers to talk to one another
	Eg:  &nbsp; <em>Facebook has an api that can be used to use some facebook data in our application.</em>

<strong> As per wikipedia:</strong>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; An application programming interface (API) is an interface or communication protocol between a client and a server intended to simplify the building of client-side software. 

- It has been described as a “contract” between the client and the server, such that if the client makes a request in a specific format, it will always get a response in a specific format or initiate a defined action.

- An API may be for a web-based system, operating system, database system, computer hardware, or software library.

#### Web API:
	Generally communicates via HTTP
	1. Twitter API
	2. Facebook API
	3. Weather API
	4. Reddit API
	5. GooglePlaces API

- **`If this then that`**:-&nbsp; It is a website that has visual interface to connect api to do thing for you.

- **`Programmable Web`**:- &nbsp; It is website, it also has lots of apis.

## XML and JSON

### Data Format

- When we use the internet, we make HTTP request and get HTML back

- API's don't respond with HTML. HTML contains information about the structure of a page. API's respond with data, not structure.

- They use simpler data formats like XML and JSON

### XML

**`XML`** => **`Extended Markup Language`**
- XML is syntactically similar to HTML, but it does not describe presentation like HTML does.

### Example:
```xml
<!-- You can put whatever tag you want-->
<person>
	<age>20</age>
	<name>Travis</name>
	<city>Los Angeles</city>
</person>
```
### JSON

**`JSON`** => **`Javascript Object Notation`**
- JSON looks exactly like JavaScript objects, is a `lightweight format for storing and transporting data`.
```json
{
	"person": {
		"age": "21",
		"name": "Travis",
		"city": "Los Angeles"
	}
}
```
- `JSON is better than XML`, you convert it quickly.

- You can use extension called JSONView in chrome to view ordered JSON

## API Request with Node

- We can use a `request package` in Node to fetch data from API through request.

- You can also get request from command line using =>  `curl url`

- For `parsing the data` that we have `received` from http request to the url, we use `JSON.parse(body)`.

```javascript
// /app.js

var request = require("request");

request("http://google.com", function(error, response, body){
	if (!error && response.statusCode == 200){
		console.log(body); // body contains the HTML for the Google Homepage
	}
});

// We have a callback function in the request because the process 
// of requesting is not instantaneous it need time, sometime it may 
// hungup(when sever is down and such cases).

// error --> it will hold any error
```

Another way of doing that is</strong>
```javascript
// /app.js

resquest("http://google.com", function(error, response, body){
	if(error){
		console.log("Something went wrong");
		console.log(error)
	} else {
		if(response.statusCode == 200){
			console.log(body);
		}
	}
});

// We can also use API urls

// Here we are requesting for something that doesn't exist
resquest("http://hsduhfdh.com", function(error, response, body){
	if(error){
		console.log("Something went wrong");
		console.log(error)
	} else {
		if(response.statusCode == 200){
			console.log(body);
		}
	}
});
```
<strong>Remember that the data we get from an API is mostly in JSON format</strong>
- so we cannot use it until we convert it into JavaScript Object.

- For this we have to parse it and JavaScript comes with built-in method => `JSON.parse(body)`.
```javascript
// app.js

request("url of api", function(error, response, body){
	if (!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
	}
});
```
## JSON Placeholder API

- It is a simple API, that has bunch of dummy data
- They have a bunch of resources, we will be using the /users

```javascript
// /app.js

// const => constant, that variable will remain constant
const request = require(request);

// Locus is a debugging module which allows you to execute 
// commands at runtime via a REPL(read–eval–print loop).

// eval() is a global function in JavaScript that evaluates 
// a specified string as JavaScript code and executes it

// To install locus -> npm i -D locus
// For windows to install locus --> npm install -D locus@2.0.0
// -D - development
// -S - Save

const URL = "https://jsonplaceholder.typicode.com/users/1";
request(URL, function(error, response, body){
	eval(require(locus)) // Once the use of this line is done then remove this line
	// What locus does that it freezes the code in the line where it is 
	// written and then we have access to any of the variable that are 
	// available to us, so we have access to request variable, 
	// error variable, response variable and body variable.
	 
	if(!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
		console.log(parsedData);
	}
});
```


## Better Way of Concatenation
```javascript
request(URL, function(error, response, body){ 
	if(!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
		// console.log(parsedData.name + " lives in " + parsedData.address.city);
		console.log(`${parsedData.name} lives in ${parsedData.address.city}`);
	}
});
```

##  Arrow Functions

- Arrow Function (=>), instead of writing function we can use "=>" sign.
```javascript
request(URL, (error, response, body) => { 
	if(!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
		console.log(`${parsedData.name} lives in ${parsedData.address.city}`);
	}
});

// Arrow function syntax
// (parameters of a function) => {
//		function code block
//	}
```

## Using Promises

<strong>As per GeeksforGeeks</strong>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A Promise in Node means `an action which will either be completed or rejected`. In case of completion, the promise is kept and otherwise, the promise is broken. So as the word suggests either the `promise is kept or it is broken`. And `unlike callbacks, promises can be chained`. Callbacks to Promises.

- There is package named `request-promise` for this thing.
- For this you have to install the `request` and the `request-promise`

```javascript
// /app.js

const rp = require("request-promise");


// rp.("http/www.google.com")
//	  .then(function(htmlstring){
// 			// Process html...
// 	   })
// 	  .catch(function(err){
// 			// Crawling failed
// 		});

rp("https://jsonplaceholder.typicode.com/users/1")
	.then((htmlstring) => {
		console.log(htmlstring);
	})
	.catch((err) => {
		console.log("Error", err);
	});


rp("https://jsonplaceholder.typicode.com/users/1")
	.then((body) => {
		const parsedData = JSON.parse(body);
		console.log(`${parsedData.name} lives in ${parsedData.address.city}`)
	})
	.catch((err) => {
		console.log("Error", err);
	});
```
<hr>

### Done!!!

<strong> As per wikipedia:</strong>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; An application programming interface (API) is an interface or communication protocol between a client and a server intended to simplify the building of client-side software. 

- It has been described as a “contract” between the client and the server, such that if the client makes a request in a specific format, it will always get a response in a specific format or initiate a defined action.

- An API may be for a web-based system, operating system, database system, computer hardware, or software library.

#### Web API:
	Generally communicates via HTTP
	1. Twitter API
	2. Facebook API
	3. Weather API
	4. Reddit API
	5. GooglePlaces API

- **`If this then that`**:-&nbsp; It is a website that has visual interface to connect api to do thing for you.

- **`Programmable Web`**:- &nbsp; It is website, it also has lots of apis.

## XML and JSON

### Data Format

- When we use the internet, we make HTTP request and get HTML back

- API's don't respond with HTML. HTML contains information about the structure of a page. API's respond with data, not structure.

- They use simpler data formats like XML and JSON

### XML

**`XML`** => **`Extended Markup Language`**
- XML is syntactically similar to HTML, but it does not describe presentation like HTML does.

### Example:
```xml
<!-- You can put whatever tag you want-->
<person>
	<age>20</age>
	<name>Travis</name>
	<city>Los Angeles</city>
</person>
```
### JSON

**`JSON`** => **`Javascript Object Notation`**
- JSON looks exactly like JavaScript objects, is a `lightweight format for storing and transporting data`.
```json
{
	"person": {
		"age": "21",
		"name": "Travis",
		"city": "Los Angeles"
	}
}
```
- `JSON is better than XML`, you convert it quickly.

- You can use extension called JSONView in chrome to view ordered JSON

## API Request with Node

- We can use a `request package` in Node to fetch data from API through request.

- You can also get request from command line using =>  `curl url`

- For `parsing the data` that we have `received` from http request to the url, we use `JSON.parse(body)`.

```javascript
// /app.js

var request = require("request");

request("http://google.com", function(error, response, body){
	if (!error && response.statusCode == 200){
		console.log(body); // body contains the HTML for the Google Homepage
	}
});

// We have a callback function in the request because the process 
// of requesting is not instantaneous it need time, sometime it may 
// hungup(when sever is down and such cases).

// error --> it will hold any error
```
<strong>Another way of doing that is</strong>
```javascript
// /app.js

resquest("http://google.com", function(error, response, body){
	if(error){
		console.log("Something went wrong");
		console.log(error)
	} else {
		if(response.statusCode == 200){
			console.log(body);
		}
	}
});

// We can also use API urls

// Here we are requesting for something that doesn't exist
resquest("http://hsduhfdh.com", function(error, response, body){
	if(error){
		console.log("Something went wrong");
		console.log(error)
	} else {
		if(response.statusCode == 200){
			console.log(body);
		}
	}
});
```
<strong>Remember that the data we get from an API is mostly in JSON format</strong>
- so we cannot use it until we convert it into JavaScript Object.

- For this we have to parse it and JavaScript comes with built-in method -> `JSON.parse(body)`.
```javascript
// app.js

request("url of api", function(error, response, body){
	if (!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
	}
});
```
## JSON Placeholder API

- It is a simple API, that has bunch of dummy data
- They have a bunch of resources, we will be using the /users

```javascript
// /app.js

// const => constant, that variable will remain constant
const request = require(request);

// Locus is a debugging module which allows you to execute 
// commands at runtime via a REPL(read–eval–print loop).

// eval() is a global function in JavaScript that evaluates 
// a specified string as JavaScript code and executes it

// To install locus -> npm i -D locus
// For windows to install locus --> npm install -D locus@2.0.0
// -D - development
// -S - Save

const URL = "https://jsonplaceholder.typicode.com/users/1";
request(URL, function(error, response, body){
	eval(require(locus)) // Once the use of this line is done then remove this line
	// What locus does that it freezes the code in the line where it is 
	// written and then we have access to any of the variable that are 
	// available to us, so we have access to request variable, 
	// error variable, response variable and body variable.
	 
	if(!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
		console.log(parsedData);
	}
});
```


## Concatenation Better Way
```javascript
request(URL, function(error, response, body){ 
	if(!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
		// console.log(parsedData.name + " lives in " + parsedData.address.city);
		console.log(`${parsedData.name} lives in ${parsedData.address.city}`);
	}
});
```

##  Arrow Functions

- Arrow Function (=>), instead of writing function we can use "=>" sign.
```javascript
request(URL, (error, response, body) => { 
	if(!error && response.statusCode == 200){
		var parsedData = JSON.parse(body);
		console.log(`${parsedData.name} lives in ${parsedData.address.city}`);
	}
});

// Arrow function syntax
// (parameters of a function) => {
//		function code block
//	}
```

## Using Promises

<strong>As per GeeksforGeeks</strong>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A Promise in Node means `an action which will either be completed or rejected`. In case of completion, the promise is kept and otherwise, the promise is broken. So as the word suggests either the `promise is kept or it is broken`. And `unlike callbacks, promises can be chained`. Callbacks to Promises.

- There is package named `request-promise` for this thing.
- For this you have to install the `request` and the `request-promise`

```javascript
// /app.js

const rp = require("request-promise");


// rp.("http/www.google.com")
//	  .then(function(htmlstring){
// 			// Process html...
// 	   })
// 	  .catch(function(err){
// 			// Crawling failed
// 		});

rp("https://jsonplaceholder.typicode.com/users/1")
	.then((htmlstring) => {
		console.log(htmlstring);
	})
	.catch((err) => {
		console.log("Error", err);
	});


rp("https://jsonplaceholder.typicode.com/users/1")
	.then((body) => {
		const parsedData = JSON.parse(body);
		console.log(`${parsedData.name} lives in ${parsedData.address.city}`)
	})
	.catch((err) => {
		console.log("Error", err);
	});
```
<hr>

### Done!!!
