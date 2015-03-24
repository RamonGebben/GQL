

## GQL

A wrapper to do some easy logging and writing stuff to Google Spreadsheets


## Setup

First you must create a doc to write to.
Open your [Google Drive](https://drive.google.com/drive/#my-drive) and sign in.

Next we need to configure the `config.json`. The config expects the following values to be present:

```json
"auth": {
	"username": "example@gmail.com",
	"password": "password"
},
"sheet": {
	"name": "name of sheet",
	"id": "optional id",
	"worksheet": {
		"name": "worksheet name",
		"id": "optional id"
	}
}
```

The `sheet.id` and `sheet.worksheet.id` are optional. 
These will appear in the console when not given, after that you could add them to make the connection faster.


## Mapping the sheet

Because Google sheets provides us with a json we can't really work with the data gets remapped.
You can ajust the names of the columns in the `config.json`. A mapping would look like this:


```json
"mapping": {
	"columns":[
		["1", "brand"],
		["2", "model"],
		["3", "customer"],
		["4", "status"],
		["5", "due_date"]
	]
}
```

##### How we receive the data
```json
{
	"1": {
	    "1": "motorola",
	    "2": "moto g",
	    "3": "t-mobile-usa",
	    "4": "IN",
	    "5": "31-10-2015"
	}
}
```
##### After mapping
```json
{
    "brand": "motorola",
    "model": "moto g",
    "customer": "t-mobile-usa",
    "status": "IN",
    "due_date": "31-10-2015",
    "id": 1
}

```

## Actions

The action have a very simple syntax:

```javascript
GQL.name_of_action( [id], data, function(){
	//Gets executed when done.
});

```

Although some action may require an id like:

- GET
- UPDATE
- DESTROY

Because else we wouldn't not know which record to update.


#### GET

The `GET` action requires an id or an query to find a array of records to match the query.

```javascript
// With an ID
GQL.get( id, data, function(){
	//Gets executed when done.
});
// => {} returns an Object

// With an query
// NOTE: Only `===` is supported at this point
GQL.get( 'brand === motorola', data, function(){
	//Gets executed when done.
});
// => [{}, {}, {}] returns an Array of Objects

```

#### CREATE

Adds a new record field to the db and updates it with the data that is given.

```javascript
GQL.create( data, function(){
	//Gets executed when done.
});

```
#### UPDATE

Update a record.

```javascript
GQL.update( id, data, function(){
	//Gets executed when done.
});

```
#### DESTROY

Removal is not possible. The record field will be made empty so count as inactive.

```javascript
GQL.destroy( id, function(){
	//Gets executed when done.
});

```

#### DB

If you want to provide al the info to a client side application you can just dump the entire db.

```javascript
GQL.db() // return entire db
```