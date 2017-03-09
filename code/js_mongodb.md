# mongoDB

- free hosting
- how to use
  - connect using the mongodb url
  - mongoose.Schema
  - mongoose.model
  - save a record
  - retrieve a model
- bson

----

## free hosting

[mlab](https://mlab.com) hosts 0.5GB mongodb databases for free

----

## how to use

there is an npm package called [mongoose](https://www.npmjs.com/package/mongoose) which is good for connecting mongodb databases with node.

##### import mongoose and connect using the mongodb url

```node
const mongoose = require('mongoose')

mongoose.connect('mongodb://<dbuser>:<dbpassword>@url:port/directory')
```

##### mongoose.Schema lets us create a new model structure

```node
const Schema = mongoose.Schema
const personSchema = new Schema({
  firstname: String,
  lastname: String,
  username: String,
})
```

##### mongoose.model lets us create a kind of constructor function for the model using the schema we created

```node
const Person = mongoose.model('Person', personSchema)

// we can now create a new record
const bob = Person({
  firstname: 'bob',
  lastname: 'smith',
  username: 'bobsmith666'
})
```

##### save a record in the model

```node
bob.save(err => {
  if (err) throw err
  console.log('saved')
})
```

##### retrieve a model

```node
Person.find({}, (err, data) => {
  if (err) throw err
  console.log(data)
})
```

----

## bson

mongodb stores data in its own **bson** (Binary JSON) format

each document contains both a record and also its structure. this allows you to reorganise data on the fly and means you're not restricted to the schema you used initially.
