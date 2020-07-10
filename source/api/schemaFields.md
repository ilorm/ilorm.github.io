# SchemaField
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
