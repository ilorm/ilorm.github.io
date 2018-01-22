# API Public

## Ilorm
Main package documentation.
```javascript
const ilorm = require('ilorm');
```

### Exported class
#### Ilorm.Model
[See Model](#model)
```javascript
const { Model } = require('ilorm');
```

#### Ilorm.Query
[See Query](#query)
```javascript
const { Query } = require('ilorm');
```

#### Ilorm.Schema
[See Schema](#schema)
```javascript
const { Schema } = require('ilorm');
```

### Exported functions
#### Ilorm.declareModel()
#### Ilorm.newModel()
Create a new Model class
```javascript
const { newModel } = require('ilorm');

const Model = newModel({ name, schema, connector })
```
Return a class Model you can use in your project to create new data or query.

| Parameter        | Type    | Default | Description              |
|:----------------:|:-------:|:-------:| ------------------------ |
| name | String, Symbol | Symbol('model') | The unique name of the model, could be use to [reference](#schemafieldreference) object. |
| schema | [Schema](#schema) | none | Specify the schema associated with the given model. |
| connector | [Connector](#connector) | none | Specify the connector to use with the given model |

!!! Tip
    You can extends the created Model to add specific behavior of your code.
    ```javascript
    class YourModel extends newModel(conf) {}
    ```
    
    ??? example "Example of extends Model"
        ```javascript
        class User extends newModel(userConf) {
            static queryMale() {
                return super.query()
                    .gender.is('M');
            }
        }
        ```

??? example "Example of newModel"
    Full example of creating a UserModel
    ```javascript hl_lines="12 13 14 15 16"
    const { Schema, newModel } = require('ilorm');
    const mongoConnector = require('./mongoConnector');

    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().required(),
    });
    
    const UserModel = newModel({
        name: 'users',
        schema: userSchema,
        connector: mongoConnector,
    });
    ```
#### Ilorm.use()

## Model

### <small style="color:blue;">(static)</small> Model.query()
Create a [Query](#query) instance targeting the current Model.

```javascript
const query = Model.query();
```
Return a query instance linked with the current Model.

??? example "Example of query"
    UserModel is a Model with a numberField : `weight`.
    ```javascript hl_lines="1"
    const user = async UserModel.query()
        .weight.min(80)
        .findOne();
    
    ```

### <small style="color:blue;">(static)</small><small style="color:red;">(async)</small> Model.getById()
Get an instance of the model by it's Id.
```javascript
const modelInstance = await Model.getById(id);
```
Return the instance associated with the given id.

| Parameter        | Type    | Default | Description              |
|:----------------:|:-------:|:-------:| ------------------------ |
| id | Mixing |  | The id of the instance to get. |


### <small style="color:red;">(async)</small> Model.save()
Save the current instance into the database.
- If the instance exists, it will make an update with the updated field only.
- If the instance does not exists. It will create the instance into the database.

```javascript
await modelInstance.save();
```

### <small style="color:red;">(async)</small> Model.remove()
Remove the current instance from the database.

```javascript
await modelInstance.remove();
```

## Query

### <small style="color:red;">(async)</small> Query.findOne()
Execute the query, and returns one element which match it.

```javascript
const instance = await query.findOne();
```
Return one instance which match the query.

### <small style="color:red;">(async)</small> Query.find()
Execute the query, and returns one array containing all elements which match the query.

```javascript
const arrayOfInstances = await query.find();
```
Return all instance which match the query.

### <small style="color:red;">(async)</small> Query.stream()
Execute the query, and returns the stream of all elements matching the query

```javascript
const streamOfInstances = await query.stream();
```
Return a stream containing all elements matching the query.

### <small style="color:red;">(async)</small> Query.count()
Execute the query, and return how many item match the query.

```javascript
const numberOfInstance = await query.count();
```
Return the number of instance which match the query.

### <small style="color:red;">(async)</small> Query.updateOne()

### <small style="color:red;">(async)</small> Query.update()

### <small style="color:red;">(async)</small> Query.removeOne()

### <small style="color:red;">(async)</small> Query.remove()

## Schema
Schema define how your data are stored.

??? example "Example of schema"
    Basic example of what a schema "user" could look like :
    ```javascript
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().required(),
    });
    ```

### constructor Schema()
Instantiate a new schema object.

```javascript
new Schema(schemaDefinition);
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| schemaDefinition | Object  | An object ; `{ key: SchemaField }` where every key will be the name of the field in the model, and SchemaField the definition of the field. |


### <small style="color:blue;">(static)</small> Schema.boolean()
Factory to create a [Boolean](#schemafieldboolean).
```javascript
Schema.boolean()
```

Return a [Boolean instance](#schemafieldboolean).

??? example "Example of boolean"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="8"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().required(),
    });
    ```
### <small style="color:blue;">(static)</small> Schema.date()
Factory to create a [Date](#schemafielddate).
```javascript
Schema.date()
```

Return a [Date instance](#schemafielddate).

??? example "Example of date"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="6"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().required(),
    });
    ```

### <small style="color:blue;">(static)</small> Schema.number()
Factory to create a [Number](#schemafieldnumber).
```javascript
Schema.number()
```

Return a [Number instance](#schemafieldnumber).

??? example "Example of number"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="7"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().required(),
    });
    ```
    
### <small style="color:blue;">(static)</small> Schema.reference()
Factory to create a [Reference](#schemafieldreference).
```javascript
Schema.reference(modelName)
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| modelName | String  | Specify which model is linked with the given reference. |


Return a [Reference instance](#schemafieldreference).

??? example "Example of reference"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="6"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      father: Schema.reference('users'),
    });
    ```
    
### <small style="color:blue;">(static)</small> Schema.string()
Factory to create a [String](#schemafieldstring).
```javascript
Schema.string()
```

Return a [String instance](#schemafieldstring).

??? example "Example of string"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="4 5"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().required(),
    });
    ```

## SchemaField
SchemaField is a class used to define the behavior of your fields. Every field type
are children of the SchemaField class. The SchemaField class is an abstract class.
You can create your own custom field by inheritance with SchemaField.

### SchemaField.default()
Specify the default value of the field, if no value are provided.
```javascript
const field = new SchemaField();

field.default(value);
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | *  | Define the default value of the field. |

??? example "Example of default"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="8"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().default(true).required(),
    });
    ```

### SchemaField.required()
Specify the field as required. A required field is mandatory to be save.
An instance without mandatory field throw error.
```javascript
const field = new SchemaField();

field.required(isRequired = true);
```

| Parameter        | Type    | Default | Description              |
|:----------------:|:-------:|:-------:| ------------------------ |
| isRequired | Boolean  | true | Define the field as required (or not). |

Per default, the field is always not required. By calling this method, without parameter
you change the required status.

??? example "Example of required"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="4 5 8"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
      birthday: Schema.date(),
      weight: Schema.number(),
      isLogged: Schema.boolean().default(true).required(),
    });
    ```

## SchemaField/Boolean
Define the field as boolean.

## SchemaField/Date
Define the field as date.

## SchemaField/Number
Define the field as number.

## SchemaField/Reference
Define the field as reference.

## SchemaField/String
Define the field as string.

