# Basics
- [legacy app brewery mongodb tutorial](https://appbrewery.com/courses/legacy-complete-web-development-course/lectures/46570235)
	- [tutorial on installation (medium article) ](https://medium.com/@LondonAppBrewery/how-to-download-install-mongodb-on-windows-4ee4b3493514)
	- [official docs](https://www.mongodb.com/docs/)
- Data directory
	- C:\Program Files\MongoDB\Server\8.0\data\
- [downloading mongosh](https://stackoverflow.com/questions/73429283/badvalue-error-no-args-for-configdb-try-c-program-files-mongodb-server-6-0) - u need to download this separately to use `mongosh` command
- A database in MongoDB
	- container for collections
- A collection in MongoDB
	- group of MongoDB documents (a single data record) within a database
	- like a **table** in relational databases, but it doesn’t require a fixed schema
	- can have multiple collections within a database
- What is `_id`
	- a special, unique identifier for documents in a collection
	- By default, every document in a MongoDB collection must have an **`_id`** field, and it serves as the primary key for that document
- commands
	- `mongod` 
		- starts the mongodb server. Runs in the bg, managing databses and handling client connections
	- `mongosh` 
		- connects to the server, lets u interact with the database, execute queries, and manage data
		- makes u ready to use the mongo shell
# Mongo shell
mongo shell commands
- `help`
- `db`
	- shows which db you're in
- `show dbs` - check what dbs we already have
	- comes preloaded with 3 databases (normal to occupy some disk space even if you haven't added any data)
	- `admin`  - Stores authentication and user-related data. Used for administrative tasks
	- `config`  - Stores sharding metadata (if sharding is used).
	- `local`  - Stores replication-related data. It is not replicated across servers.
- `use newDatabase`
	- switches. if doesn't exist, creates new db
	- to be listed in `show dbs` it needs to have some content first
- `db.dropDatabase()`
	- deletes database
	- U need to `use` it first
- `show collections`
	- shows collections for current db
- `db.collection.drop()`
	- drops a collection in current db

### CRUD operations
- [docs](https://www.mongodb.com/docs/manual/crud/)
- create, read, update, delete
- Create data
	- `db.collection.insertOne({})`
	- `db.collection.insertMany()`
	- will create the collection if it doesn't exist
- Read data
	- `db.collection.find()`
		- shows all records in the collection
	- `db.collection.find(query, projection, options)`
		- all optional params
		- id always comes by default
		- query - a key value pair
			- often with selectors: ex)`$gt` (greater than specified value), `$lt` (lesser than specified value) etc
		- projection
			- specifies field to return
			- ex) `{name: 1, address:0}` - 1 means true, 0 false
		- read more in docs
- Update data
	- `db.collection.updateOne({update filter}, {update action})`
		-  update filter - which document u wanna update, ex: `{_id:1}`
		- update action - ex: `$set: {age: 42}`
	- `updateMany()`, `replaceOne()`
- Delete data
	- `db.collection.deleteOne({delete filter})`
	- `db.collection.deleteMany()`

```mongosh
/// creating data
db.products.insertOne({_id: 1, name:"pen", price: 1.20})

/// reading data
db.products.find()
db.products.find({name: 'pen'}) // query param
db.products.find({price: {$gt: 1}})
db.products.find({_id: 1}, {name: 1}) // with projection

/// updating data
db.products.updateOne({_id:2}, {$set: {stock:20}})

/// deleting data
 db.products.deleteOne({_id:1})
db.staff.deleteMany({age : {$gt: 25}})
db.posts.deleteOne({ _id: ObjectId('67ea38e5a134f0ffb920f2e3') })

```

# Relationships in MongoDB

### Embedded Documents (Denormalization)
- every document is represented by curly braces
- so you can have an embedded document inside a document
	- "Documents inside documents"
- best for one-to-few relationships
- **Good for fast reads** but **bad if the inner document grow too large**
```json
{
  "_id": 1,
  "title": "My First Blog Post",
  "content": "This is my post...",
  "comments": [
    { "user": "Alice", "text": "Great post!" },
    { "user": "Bob", "text": "Nice work!" }
  ]
}
```

### Referenced Documents (Normalization) 
- Instead of embedding, you store **references (IDs)** to related documents in other collections.
	- "Storing only IDs"
- Useful when data is **reused across multiple documents** or **grows large**.
- Best for **one-to-many** and **many-to-many** relationships.

```json
== POST COLLECTION ==
{
  "_id": 1,
  "title": "My First Blog Post",
  "content": "This is my post...",
  "comments": [1001, 1002]  // References to comment documents
}

== COMMENTS COLLECTION ==
{
  "_id": 1001,
  "post_id": 1,
  "user": "Alice",
  "text": "Great post!"
}
{
  "_id": 1002,
  "post_id": 1,
  "user": "Bob",
  "text": "Nice work!"
}
```

# Mongoose
>[!Mongoose]
>- An Object Document Wrapper
>- Allows your [[Node.js]] project (that speaks in [[== JavaScript ==|JS]]) to talk to your mongodb database
>- Main objective is simplify the validation, casting, and business logic boilerplate

- https://mongoosejs.com/

#### Basics
```js
const mongoose = require("mongoose");

// connect
async function connectDB() {
  await mongoose.connect("mongodb://127.0.0.1:27017/test");
}

connectDB().catch((err) => console.log(err));

// creating a schema
const BlogSchema = new mongoose.Schema({
  title: String,
  body: String,
  category: String,
});

// creating a model
const Blog = mongoose.model("Blog", BlogSchema, "blog");

// creating 1 post from the model
const post1 = new Blog({ title: "testing title", body: "", category: "test" });

post1
  .save()
  .then(() => {
    console.log("Post saved successfully!");
  })
  .catch((err) => console.log("Error saving post:", err));

// close connection
mongoose.connection.close();
```

-  `await mongoose.connect("mongodb://127.0.0.1:27017/test");`
	- connects to `test` database, if it doesn't exist, create one
- `const Blog = mongoose.model("Blog", BlogSchema, "blog");`
	- You’re creating a **model** (called `Blog`) that you can then use to **create, update, or query documents** in the `blog` collection, and saving the model into a variable named `Blog`
	- If you omit the third parameter, Mongoose will automatically pluralize the model name and use that as the collection name
	- Mongoose will use "blogs" collection
- `post1.save()`
	- saves into collection
- `model.insertMany(1st, 2nd)`
	- `1st` - array of objects we want to acc
	- `2nd` - callback (like errors)
	- saves multiple at once into collection
- good idea to close connection once you're done with it

#### Reading from db
```js
async function fetchData() {
	const data = await Blog.find({});
  
    // can also accept a callback
	const data = await Blog.find(function(err, res){
		if (err) {
		  console.log(err)
		} else {
		  console.log(res)
		}
	})
	  console.log(data);
	}

fetchData();
```
- `const data = await Blog.find({});`
	- Fetch all documents in the "blog" collection
	- `{}` is an empty filter
		- ex of non-empty: `find({ category: "test" })`

#### Validation check
- mongoose documentation > validation
- built in validators
	- all has built-in `required` validator
	- Numbers has `min/max` validators
	- Strings have `enum, match, minLength, maxLength` validators
```js
const fruitSchema = new mongoose.Schema({
  name: {
	  type: String,
	  required: [true, 'Why no name?']
  },
  rating: {
	  type: Number,
	  min:1,
	  max:10
  }
  review: String,
});
```
#### Updating/deleting data
Update
```js
async function updateData() {
  try {
    const result = await Blog.updateOne(
      { _id: "67e63871a734319ef85846e4" }, //item to be updated
      { title: "changing title2 again" }
    );
    console.log(result);
  } catch (err) {
    console.log(err);
  }
}
updateData();
```
- `Mode.updateOne()`
	- params: `filter, update, <options>`

Delete
```js
async function deleteData() {
  try {
    const res = await Blog.deleteOne(
	    { _id: "67e63871a734319ef85846e4" }
    );
    console.log(res);
  } catch (err) {
    console.log(err);
  }
}
deleteData();
```
- `deleteMany()`

#### Establishing relationships & embedding documents

```js
const fruitSchema = new mongoose.Schema({
  name: String,
  rating: {
    type: Number,
    min: 1,
    max: 10,
  },
});

const personSchema = new mongoose.Schema({
  name: String,
  age: Number,
  favoriteFruit: fruitSchema,
});

const Fruit = mongoose.model("Fruit", fruitSchema);
const pineapple = new Fruit({
  name: "Pineapple",
  rating: 9,
});

const Person = mongoose.model("Person", personSchema);
const person = new Person({
  name: "John",
  age: 30,
  favoriteFruit: pineapple,
});

pineapple
   .save()
   .then(async () => {
     const data = await Fruit.find({});
     console.log(data);
   })
   .catch((err) => console.log(err));

person
  .save()
  .then(async () => {
    const data = await Person.find({});
    console.log(data);
  })
  .catch((err) => console.log(err));
```
- person has embedded document (the pineapple)