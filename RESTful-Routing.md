# RESTful Routing

	1. REST - Representational State Transfer
	2. It is just a pattern for definig our routes. 
	3. Its way of mapping HTTP routes and CRUD(Create Read Update Delete).

## Table of 7 RESTful Routes


| Name | Path  | HTTP verb  | Purpose  | Mongoose Method  |
|---|---|---|---|---|
| INDEX  | /dogs  | GET  | Display a list of all dogs  | Dog.find()  |
|  NEW | /dogs/new  |  GET | Display form to make new dog    | N/A  |
|  CREATE | /dogs  | POST  | Add new to DB, then redirect to somewhere	  | Dog.create()  |
|  SHOW |  /dogs/:id | GET  |  Show info about one specific dog  	  |  Dog.findById() |
| Edit  | /dogs/:id/edit  | GET  |   Show edit form for one dog 	  | Dog.findById()   |
| Update  | /dogs/:id  | PUT  | Update a particular dog, then redirect to somewhere  | Dog.findByIdAndUpdate()  |
| Destroy  | /dogs/:id  | DELETE  | Delete a particular dog, then redirect somewhere	  |  Dog.findByIdAndRemove() |

<hr>

### Done!!!
