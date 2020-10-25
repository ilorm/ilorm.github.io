# ilorm-connector-knex

The Knex connector is the Connector to use knex compatible database as storage for your
model.

## Features
- Support all Knex supported database


## How it works
To use this connector :

- first declare it in your project :
```javascript
const ilorm = require('ilorm');
const ilormKnex = require('ilorm-connector-knex');

ilorm.use(ilormKnex);
```

- Second, you need to create a KnexConnector. To do that, you need to make a connection
between your database and your project. You need knex to achieve that:
```javascript
const knex = require('knex')({
  client: 'mysql2',
  connection: {
    host: '127.0.0.1',
    user: 'root',
    password: 'someSecretPassword',
    database: 'yourDb',
  },
});
const ilormKnex = require('ilorm-connector-knex');

const knexConnection = IlormKnex.fromKnex(knex);

```

- Finally you need to choose the table used to store your model :
```javascript
const modelFactoryParams = {
  name: 'users',
  schema: userSchema,
  connector:  new knexConnection({ sourceName: 'users' }),
};

const UserModel = newModel(modelFactoryParams);
```
