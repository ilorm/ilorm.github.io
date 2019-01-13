# Schema
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
Factory to create a [Boolean](../schemaFields#schemafieldboolean).
```javascript
Schema.boolean()
```

Return a [Boolean instance](../schemaFields#schemafieldboolean).

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
Factory to create a [Date](../schemaFields#schemafielddate).
```javascript
Schema.date()
```

Return a [Date instance](../schemaFields#schemafielddate).

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
Factory to create a [Number](../schemaFields#schemafieldnumber).
```javascript
Schema.number()
```

Return a [Number instance](../schemaFields#schemafieldnumber).

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
Factory to create a [Reference](../schemaFields#schemafieldreference).
```javascript
Schema.reference(modelName)
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| modelName | String  | Specify which model is linked with the given reference. |


Return a [Reference instance](../schemaFields#schemafieldreference).

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
Factory to create a [String](../schemaFields#schemafieldstring).
```javascript
Schema.string()
```

Return a [String instance](../schemaFields#schemafieldstring).

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