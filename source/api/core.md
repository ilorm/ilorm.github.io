## Ilorm
Main package documentation.
```javascript
const ilorm = require('ilorm');
```

### Exported class
#### Ilorm.Schema
Use to declare your schema. [See Schema](../schema)
```javascript
const { Schema } = require('ilorm');
```

### Exported functions
#### Ilorm.declareModel()
Declare model is used to change the Model associated with the given name. Could be used
to define which Class will be instancied in each case.

declareModel is already called in newModel, you need to invoke it, only if you overload
the resulting class of a newModel call.
```javascript
ilorm.declareModel(Model);
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| Model | [Model](../model) | The Model to associate with the given name. |

??? example "Example of declareModel"
    ```javascript hl_lines="3"
    userParams = {
       name: 'users',
       // some other definition
    };
    const BaseUser = newModel(userParams);
    
    const User extends BaseUser
    
    // Before declareModel ilorm associate 'users' with BaseUser
        
    ilorm.declareModel(User);
    
    // After ilorm associate 'users' with User, BaseUser was replaced
    
    // Now you could reference the Model User as users in your schema :
    const friendSchema = new Schema({
        userA: Schema.reference('users').required(),
        userB: Schema.reference('users').required(),
    });
    ```

#### Ilorm.newModel()
Create a new Model class
```javascript
const { newModel } = require('ilorm');

const Model = newModel({ name, schema, connector })
```
Return a class [Model](../model) you can use in your project to create new data or query.

| Parameter        | Type    | Default | Description              |
|:----------------:|:-------:|:-------:| ------------------------ |
| name | String, Symbol | Symbol('model') | The unique name of the model, could be use to [reference](../schemaFields) object. |
| schema | [Schema](../schema) | none | Specify the schema associated with the given model. |
| connector | [Connector](../../connectors/intro) | none | Specify the connector to use with the given model |

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
#### Ilorm.use(plugin)
Declare a plugin to be use by ilorm. This method allows the plugin to overload internal class used by ilorm.


