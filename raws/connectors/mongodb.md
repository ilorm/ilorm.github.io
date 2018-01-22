# ilorm-connector-mongo

The MongoDB connector is the Connector to use to mongoDB as storage for your ilorm model.

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

