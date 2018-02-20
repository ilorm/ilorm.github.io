# ilorm-connector-mongo

The MongoDB connector is the Connector to use mongoDB as storage for your ilorm model.

## Features
- Add MongoDB as connector.
- Add specifics MongoDB SchemaFields : Array, Map, Object and ObjectId.
- Add specifics MongoDB query operator : push (on Array only).
- Add Model.aggregate to build aggregation pipeline.

## How it works
To use this connector :

- first declare it in your project :
```javascript
const ilorm = require('ilorm');
const ilormMongo = require('ilorm-connector-mongodb');

ilorm.use(ilormMongo);
```

- Second, you need to create a MongoConnector. To do that, you need to make a connection
between your database and your project. You can create it with a basic mongo client :
```javascript
const ilormMongo = require('ilorm-connector-mongodb');
const { MongoClient, } = require('mongodb');
const DB_URL = 'mongodb://localhost:27017/ilorm';

const mongoClient = await MongoClient.connect(DB_URL);
const database = await mongoClient.db('ilorm');

const MongoConnector = ilormMongo.fromClient(database);
```

- Finally you need to choose the collection used to store your model :
```javascript
const modelFactoryParams = {
  name: 'users',
  schema: userSchema,
  connector: new MongoConnector({
    collectionName: 'users',
  }),
};

const UserModel = newModel(modelFactoryParams);
```
## Connector
### (constructor) Connector

## Model

## Query

## Schema 

## SchemaField
### Array
Array is used to define embed array in your Schema. You could define behavior of the 
child element.
```javascript
// To store users in a array ;
new Schema({
  users: Schema.array({
    firstName: Schema.String(),
    lastName: Schema.string()
  }),
});
```

### Map
Map is used to define Map in your Schema. A map is a key: value association.
You can define behavior of the child element (the value of the array). And the format
of your key.

### Object
Object is used to define embed Object in your Schema. You could define behavior of the 
child element. 


### ObjectId
ObjectId is the ID system used by MongoDB.
