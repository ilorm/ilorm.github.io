# Quick start

Ilorm are based on five core concepts : Connector, Model, Schema, SchemaField and Query.
- Connectors are the definition which kind of database you use to store your data.
- Model are class representing your data.
- Schema define your model behavior (fields type)
- SchemaField define the behavior of your data field.
- Query is a powerful query builder tool to edit, delete, get data...

## Install ilorm
First install ilorm :
```shell
npm install ilorm
```

After you can install the mongo-connector :
```shell
npm install ilorm-connector-mongodb
```

You can use another connector if you use different kind of database.

## Define a schema
To define your schema, just use the Schema class of ilorm
```javascript
const { Schema, } = require('ilorm');

const invoiceSchema = new Schema({
  createdAt: Schema.date(),
  amount: Schema.number()
});
```
With this code, you have a basic schema with two properties :
- createdAt which will be a date.
- amount which will be a number.

## Prepare your Connector
```javascript
const MongoClient = require('mongodb');
const ilorm = require('ilorm');
const IlormMongo = require('ilorm-connector-mongodb');

// Declare you use ilormMongo (init plugin part of the connector) :
ilorm.use(IlormMongo);

// Create a database :
const mongoClient = await MongoClient.connect(DB_URL);
const database = await mongoClient.db('ilorm');

// Create your Connector class binded with your database :
const MongoConnector = IlormMongo.fromClient(database);

// Create an instance of your connector. It will save in the collection: accounts ;
const invoiceConnector = new MongoConnector({ collection: 'invoices' });
```
Now you have an invoiceConnector, it will save the binded model in the collection `invoices`.

## Create your model
```javascript
const { newModel, } = require('ilorm');

const modelConfig = {
  name: 'invoices', // Optional, could be useful to know the model name
  schema: invoiceSchema,
  connector: invoiceConnector,
}

const Invoice = newModel(modelConfig); // Invoice Model is a class
```
Now you have all you want : A model, you can manipulate your data

## Manipulate account

??? "Find one invoice after first january 2017"
    ```javascript
    const invoice = await Invoice.query()
        .createdAt.greatherThan(new Date('2017-01-01'))
        .findOne();
    ```
    
??? "Create a new invoice"
    With attribute :
    ```javascript
    const invoice = new Invoice();
    invoice.createdAt = Date.now();
    invoice.amount = 300;
    invoice.save();
    ```
    
    Using constructor :
    ```javascript
    const invoice = new Invoice({
        createdAt: Date.now(),
        amount: 300,
    });
    invoice.save();
    ```

??? "Load an invoice per _id & remove it from database"
    ```javascript
    const invoice = await Invoice.getById(ObjectId('2088181818'));
    
    invoice.remove();
    ```

## Full example

```javascript

const MongoClient = require('mongodb');
const ilorm = require('ilorm');
const IlormMongo = require('ilorm-connector-mongodb');

const { Schema, newModel, } = ilorm;

// Declare schema :
const invoiceSchema = new Schema({
  createdAt: Schema.date(),
  amount: Schema.number()
});


// Declare you use ilormMongo (init plugin part of the connector) :
ilorm.use(IlormMongo);

// Create a database :
const mongoClient = await MongoClient.connect(DB_URL);
const database = await mongoClient.db('ilorm');

// Create your Connector class binded with your database :
const MongoConnector = IlormMongo.fromClient(database);

// Create an instance of your connector. It will save in the collection: accounts ;
const invoiceConnector = new MongoConnector({ collection: 'invoices' });

const modelConfig = {
  name: 'invoices', // Optional, could be useful to know the model name
  schema: invoiceSchema,
  connector: invoiceConnector,
}

const Invoice = newModel(modelConfig); // Invoice Model is a class
```