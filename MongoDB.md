# MongoDB

## Table of Contents:
- [Mongo-Command](#mongo-command)
- [Mongoose](#mongoose)

## Mongo Command

- mongod
- mongo
- help
- show dbs
- use
- insert 
- find
- update
- remove


## Mongoose

- mongoose is an elegant mongodb object modelling for node.js.
- mongoose is a ODM (object data mapper) what does that really means is that it's way for us to write JavaScript inside of our JavaScript files/ Express files and then JavaScript Code will interact with the database, So it's just JavaScript layer over MongoDB and it has some extra method and features.
- Then we define a Schema.
- Then we take our Schema compile it to a model which returns a object that have lot of methods.

#### NOTE: 
	1. Don't run the entire, look at each callback and what they do otherwise you will end up adding a lot data to your database. 
	2. Once you add an entity to the database comment that code block to avoid adding that same entity again and again.

```javascript
// /app.js

var mongoose = require("mongoose");

const URL = 'mongodb connection String URI';

mongoose.connect(URL, {
	useNewUrlParser: true,
	useCreateIndex: true,
	useUnifiedTopology: true
}).then(() => {
	console.log("Connected to DB");
}).catch(err => {
	console.log("Error:", err.message);
});

// Structure of cat
// This is just a pattern
var catSchema = new mongoose.Schema({
	name: String,
	age: Number,
	temprament: String
});

// When we save the pattern to a variable after compiling it to a model, it's not just a pattern now, it actually has all the methods we want. It takes the pattern and build a complex model that have all the methods
var Cat = mongoose.model("Cat", catSchema);
// The first argument (here we have put Cat) should always be the singulat version of your collection name. Since here we gave Cat it is going to make collection called Cats, there is libary that puralizes these.

// adding a new cat to database
var gerorge = new Cat({
	name: "Geroge",
	age: 11,
	temprament: "Grouchy"
});

// Making another cat
var gerorge = new Cat({
	name: "Mrs. Norris",
	age: 7,
	temprament: "Evil"
});

// Another way of adding , This is new and save at once
Cat.create({
	name: "Snow White",
	age: 15,
	temprament: "Bland"
}, function(err, cat){
	if(err){
		console.log(err);
	} else {
		console.log(cat);
	}
})

// Now when we are save a into database, there might be some issues 
// like there is no internet or mongod is not on, To know that our cat 
// is saved into the database we put a callback function inside the 
// save method.
gerorge.save(function(err, cat){
 	if(err){
 		console.log("Something went wrong");
 	} else {
		console.log("The cat is save into the database");
 		console.log(cat);
 	}
});

// retrive all cats from the database and console.log each one
Cat.find(function(err, cat){
	if(err){
		console.log("There is an error in retriving");
	} else {
		console.log("All the cats in database");
		console.log(cat);
		// Here what we get is the cat that is coming from the
		// database and not gerorge, gerorge is just in javascript
	}
});
```
<hr>

### Done!!!
