
# Node JS

## Table of contents
- [Introduction to Node](#introduction-to-node)
- [Introduction to npm](#introduction-to-npm)
- [cat-me Package](#cat-me-package)
- [knock-knock-jokes Package](#knock-knock-jokes-package)
- [faker Package](#faker-package)

#### As per Node:
	1. Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.

	2. Node.js uses an event driven, non-blocking I/O model that makes it lightweight and efficient.

	3. Node.js's package ecosystem, npm, is the largest ecosystem of open source libaries in the world
	
**`Node is how we run JavaScript on the server side.`**

- Reasons of People liking it:
	- Great Libraries
	- JavaScript
	- Npm
	- High-Performance
	- Open Source
	- Great for API's
	- Great Community
	- Asynchronous
	- Great for realtime apps
	- Great for command line utilities
	
#### Using Node
- Node Console:
	- It has its own version of console, it is all based on command line.
	- You can write JavaScript `SOME` command there.
	- You can't do something like alert("hello").
	- Because alert comes with the browser, we are not writing in browser, our command is running in some server side where there is no browser.
	- Similar DOM's, document, events then don't exist here, they are in the browser only.
	- Now we are working in the server side, BackEnd
	
	- Start by writing on command line - node
	- Running a javascript file with node - node filename.js

## Introduction to npm
`npm is the package manager for JavaScript.`

- A large amount of packages are centralised in npm
- npm has command line tool to install packages, in command we have to write
```bash
npm install packagename
```

### Installing and Using Packages
	1. Use 'npm intall' to install a package
	2. Use 'require()' to include a pacakage
	3. The package we install will be downloaded in a directory name 'node_module'
	4. Using require: var Something = require("Package Name");

## cat-me Package

#### This package generates random cat figures in your console.

Installing cat-me package
```bash
npm install cat-me
```

Inside a javascript file
```javascript
// Inside app.js

var Cat = require('cat-me');
console.log(Cat());
```
To run this javascript file run the below line in console
```bash
node app.js
```

## knock-knock-jokes Package

#### This package generates random jokes

Installing knock-knock-jokes package
```bash
npm install knock-knock-jokes
```

Inside a javascript file
```javascript
// Inside app.js

var jokes = require('knock-knock-jokes');
console.log(jokes);
```
To run this javascript file run the below line in console
```bash
node app.js
```

## faker Package

#### This package generate massive amounts of fake data in the browser and node.js.

Installing faker package
```bash
npm install faker
```

Inside a javascript file
```javascript
// Inside app.js

var faker = require('faker');

// This will create 10 random product name and prices.
for(var i=0; i<10; i++){
	console.log(faker.fake("{{commerce.productName}} and {{commerce.price}}"));
}
```
To run this javascript file run the below line in console
```bash
node app.js
```

<hr>

### Done!!!
