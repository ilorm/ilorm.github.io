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
#### Ilorm.declareModel
#### Ilorm.modelFactory
#### Ilorm.use

## Model

### <small style="color:blue;">(static)</small><small style="color:red;">(async)</small> Model.getById()

### <small style="color:red;">(async)</small> Model.save()

### <small style="color:red;">(async)</small> Model.remove()

### Model.query()

## Query

### <small style="color:red;">(async)</small> Query.findOne()

### <small style="color:red;">(async)</small> Query.find()

### <small style="color:red;">(async)</small> Query.stream()

### <small style="color:red;">(async)</small> Query.count()

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

