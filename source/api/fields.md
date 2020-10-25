# Field
Field is a class used to define the behavior of your fields. Every field type
are children of the Field class. The Field class is an abstract class.
You can create your own custom field by inheritance with Field.

### Field.default()
Specify the default value of a field. Any instance bound with the default field will be initialized with the default
value. If a function is specified, the function will be invoked and the result of it will be used as default value.
```javascript
const field = new Field();

field.default(value);
```

| Parameter        | Type    | Description              |
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
      createdAt: Schema.date().default(() => Date.now),
      isLogged: Schema.boolean().default(true).required(),
    });
    ```

### Field.required()
Specify the field as required. A required field is mandatory to be saved.
An instance with unsetted mandatory field will throw an error if save is called.
```javascript
const field = new Field();

field.required(isRequired = true);
```

| Parameter        | Type    | Default | Description              |
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

### Field.primary()
Define this field as a primary key of the linked Schema.

??? example "Example of primary"
    Basic example of what a schema "user" could look like :
    ```javascript hl_lines="4 5 8"
    const { Schema } = require('ilorm');
    
    const userSchema = new Schema({
      userId: Schema.string().primary()
      firstName: Schema.string().required(),
      lastName: Schema.string().required(),
    });
    ```

### Field.reference({ referencedModel, referencedField, })
Define a reference between the given schema and another schema.

| Parameter        | Type   | Description              |
|:----------------:|:-------:| ------------------------ |
| referencedModel | String  | The model referenced by the field |
| referencedField | String  | The field in the referenced model referenced by the field |

??? example "Example of reference"
    Invoice.customerId reference Customers id
    ```javascript hl_lines="4 5 8"
    const { Schema } = require('ilorm');
    
    const invoiceSchema = new Schema({
      invoiceId: Schema.string().primary()
      customerId: Schema.string().required().reference({
        referencedModel: 'customers',
        referencedField: 'id',
      }),
      amount: Schema.amount().required(),
    });
    ```

### Field.isValid(value)
Check if value is "valid" and could be used as value for the field.
For example, an undefined value is invalid for a required field.

| Parameter        | Type   | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixed  | The value to check. |

### Field.castValue(value)
Cast the value with the given field. Using internally everytime we set a value in the field.

| Parameter        | Type   | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixed  | The value to check. |

??? example "Example of castValue"
    ```javascript hl_lines="4 5 8"
    const { Schema } = require('ilorm');
    
    const SchemaNumber = Schema.number();
    
    const resultCast = SchemaNumber.castValue('33');
    
    expect(resultCast).to.be.a('number');
    expect(resultCase).to.be.equal(33);
    ```

## Field/Boolean
Define the field as boolean.

## Field/Date
Define the field as date.

## Field/Number
Define the field as number.

## Field/Reference
Define the field as reference.

## Field/String
Define the field as string.
