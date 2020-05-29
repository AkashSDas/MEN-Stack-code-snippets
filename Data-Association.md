# Data Association

## Table of Contents
- [Intro to Association](#intro-to-association)
- [Embedding Data](#embedding-data)
- [Referencing Data](#referencing-data)
- [module.exports](#module.exports)


## Intro to Association 

- **`Define association`**:
	<em>Association will allow us to have multiple piece of data, multiple collections of database that are related to one another. </em>

- Different types of association:
	- one:one relationships
	- one:many relationships
	- many:many relationships

- How to write associations? How you will connect different models?:
	- The two different ways are:
		* Embedding Data
			- User
			- Post
		* Referencing Data

## Embedding Data

### Just making models

Using user and post as examples.

```javascript
// app.js

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

// USER - email, name
var userSchema = new mongoose.Schema({
	email: String,
	name: String
});
var User = mongoose.model("User", userSchema);

// POST - title, content
var postSchema = new mongoose.Schema({
	title: String,
	content: String
});
var Post = mongoose.model("Post", postSchema);

// Making a new user
var newUser = new User({
	email: "charlie@brown.edu",
	name: "Charlie Brown"
});

// Saving a new user
newUser.save(function(err, user){
 	if(err){
 		console.log("Error:", err);
 	} else {
 		console.log(user);
 	}
});

// Making a post
var newPost = new Post({
	title: "Reflections on Apple",
	content: "They are delicious"
});

// Saving the post
 newPost.save(function(err, post){
 	if(err){
 		console.log("Error:", err);
 	} else {
 		console.log(post);
 	}
});
```

- Making a relationship between User and Post
- One user can have many posts but one post will have only one user
- Here we have a one:many relationship
- Here the way we will accomplish this is by embedding data inside the user schema.

### Making Relationships

Here we have to first write the postSchema and then userSchema, because if we write userSchema first then it will throw an error because it (posts: [postSchema]) line hasn't seen the postSchema.

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

// POST - title, content
var postSchema = new mongoose.Schema({
	title: String,
	content: String
});
var Post = mongoose.model("Post", postSchema);

// Our data for one user will look this:
// {
// 	email: "xyz",
// 	name: "xyz",
// 	posts: [
// 		// User can have many posts
// 		{title: "xyz", content: "xyz"},
// 		{title: "xyz", content: "xyz"},
// 		{title: "xyz", content: "xyz"},
// 		{title: "xyz", content: "xyz"}
// 	]
// }

// USER - email, name
var userSchema = new mongoose.Schema({
	email: String,
	name: String,
	posts: [postSchema]
});
var User = mongoose.model("User", userSchema);

// Making a new user
var newUser = new User({
	email: "hermione@hogwart.edu",
	name: "Hermoine Granger"
});

// Adding a post to the user
newUser.posts.push({
	title: "How to bre polyjuice potion",
	content: "Just kidding. Go to potion class to learn it"
})

newUser.save(function(err, user){
	if(err){
		console.log("Error:", err);
	} else {
		console.log(user);
	}
});

// Making a post
var newPost = new Post({
	title: "Reflections on Apple",
	content: "They are delicious"
});
newPost.save(function(err, post){
	if(err){
		console.log("Error:", err);
	} else {
		console.log(post);
	}
});

User.findOne({name: "Hermoine Granger"}, function(err, user){
	if(err){
		console.log("Error:", err);
	} else {
		console.log(user);
	}
});

// Adding Another Post
User.findOne({name: "Hermoine Granger"}, function(err, user){
	if(err){
		console.log("Error:", err);
	} else {
		user.posts.push({
			title: "3 things I really hate",
			content: "Voldemort. Voldemort. Voldemort"
		});
		user.save(function(err, user){
			if(err){
				console.log("Error:", err);
			} else {
				console.log(user);
			}
		});
	}
});
```

## Referencing Data

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

// POST - title, content
var postSchema = new mongoose.Schema({
	title: String,
	content: String
});
var Post = mongoose.model("Post", postSchema);

// USER - email, name
var userSchema = new mongoose.Schema({
	email: String,
	name: String,
	posts: [
		{
		type: mongoose.Schema.Types.ObjectId,
		ref: "Post"
		}
	]
});
var User = mongoose.model("User", userSchema);

// Creating a user
User.create({
	email: "bob@gmail.com",
 	name: "Bob Belcher"
});

// Adding post independently
Post.create({
 	title: "How to cook the best burger",
 	content: "blah blah blah blah blah"
	}, function(err, post)
		  console.log(post);
});
// The above block will add a post to database independent of User

// Connecting the post to user by id
// Another post 
Post.create({
 	title: "How to cook the best burger pt. 2",
 	content: "blah blah blah blah blah"
 }, function(err, post){
 	User.findOne({email: "bob@gmail.com"}, function(err, foundUser){
 		if(err){
 			console.log("Error:", err);
 		} else {
 			foundUser.posts.push(post);
 			foundUser.save(function(err, data){
 				if(err){
 					console.log("Error", err);
 				} else {
 					console.log(data);
 				}
 			})
 		}
 	});
});

// Another post 
Post.create({
 	title: "How to cook the best burger pt. 3",
 	content: "lkfjlsjfjdjfokaojorghkwooifpokeb"
 }, function(err, post){
 	User.findOne({email: "bob@gmail.com"}, function(err, foundUser){
 		if(err){
 			console.log("Error:", err);
 		} else {
 			foundUser.posts.push(post);
 			foundUser.save(function(err, data){
 				if(err){
 					console.log("Error", err);
 				} else {
 					console.log(data);
 				}
 			})
 		}
 	});
});

// Now using the id to get the post

	email: "xyz",
	 	name: "xyz",
	 	posts: [
			12134455564,
			2343jj432m3,
			23o32kok4k5
	 	]
	}
// Our data for one user will look this:
// {
// 	email: "xyz",
// 	name: "xyz",
// 	posts: [
// 		// User can have many posts
//			12134455564, <- These are ids of the those posts associated with a particular user
//  		2343jj432m3,
//			23o32kok4k5
// 	]
// }

// Find user
// Find all posts for that user

User.findOne({email: "bob@gmail.com"}).populate("posts").exec(function(err, user){
	if(err){
		console.log("Error:", err);
	} else {
		console.log(user);
	}
});
// By doing our posts is just an array of ids but it has full posts
// findOne({email: "bob@gmail.com"}) --> finding the user
// .populate("posts") --> it will populate the field posts, lookup all the object ids and correct data and then stick it to posts array
// .exec(function(err, user){} --> it will make all happen, it is when we are executing the code
// We are not using a callback inside the query i.e. findOne({email: "bob@gmail.com"})
// It depends on what way you use Embedding data and Object Reference Id
```

## module.exports

#### Objectives
	1. Introduce module.exports
	2. Move our models into separate files

	
**`module.exports`** => allows us to break things into files and the reason we want to that is to make our code cleaner and also it makes things modular and reusable.

We can have another file in which we have our models and then we can require these models, there we just need to write these lines 
```javascript
var Post = require("./models/post"); 
var User = require("./models/user");)
```
#### Inside app.js
```javascript
// /app.js

var Post = require("./models/post");

var mongoose = require("mongoose");
var Post = require("./models/post");
var User = require("./models/user");

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

// Creating a user
User.create({
 	email: "bob@gmail.com",
 	name: "Bob Belcher"
});

// Adding post independently
Post.create({
 	title: "How to cook the best burger",
 	content: "blah blah blah blah blah"
 }, function(err, post){
 	console.log(post);
});

// Connecting the post to user by id

// Another post 
Post.create({
 	title: "How to cook the best burger pt. 2",
 	content: "blah blah blah blah blah"
 }, function(err, post){
 	User.findOne({email: "bob@gmail.com"}, function(err, foundUser){
 		if(err){
 			console.log("Error:", err);
 		} else {
 			foundUser.posts.push(post);
 			foundUser.save(function(err, data){
 				if(err){
 					console.log("Error", err);
 				} else {
 					console.log(data);
 				}
 			})
 		}
 	});
});

// Another post 
Post.create({
 	title: "How to cook the best burger pt. 3",
 	content: "lkfjlsjfjdjfokaojorghkwooifpokeb"
 }, function(err, post){
 	User.findOne({email: "bob@gmail.com"}, function(err, foundUser){
 		if(err){
 			console.log("Error:", err);
 		} else {
 			foundUser.posts.push(post);
 			foundUser.save(function(err, data){
 				if(err){
 					console.log("Error", err);
 				} else {
 					console.log(data);
 				}
 			})
 		}
 	});
});


// Now using the id to get the post

// Find user
// Find all posts for that user

User.findOne({email: "bob@gmail.com"}).populate("posts").exec(function(err, user){
	if(err){
		console.log("Error:", err);
	} else {
		console.log(user);
	}
});
// By doing our posts is just an array of ids but it has full posts
// findOne({email: "bob@gmail.com"}) --> finding the user
// .populate("posts") --> it will populate the field posts, lookup all the object ids and correct data and then stick it to posts array
// .exec(function(err, user){} --> it will make all happen, it is when we are executing the code
// We are not using a callback inside the query i.e. findOne({email: "bob@gmail.com"})

// It depends on what way you use Embedding data and Object Reference Id
```

Now making a models directory and inside that making two files user.js and post.js 

**`user.js`**
```javascript
// /models/user.js

var mongoose = require("mongoose");

// USER - email, name
var userSchema = new mongoose.Schema({
	email: String,
	name: String,
	posts: [
		{
		type: mongoose.Schema.Types.ObjectId,
		ref: "Post"
		}
	]
});

module.exports = mongoose.model("User", userSchema);

**`post.js`**
```javascript
// /models/post.js

var mongoose = require("mongoose");

// POST - title, content
var postSchema = new mongoose.Schema({
	title: String,
	content: String
});

// Like a return value of file
module.exports = mongoose.model("Post", postSchema);
```

<hr>

### Done!!!
