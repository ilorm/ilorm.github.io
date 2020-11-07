# Query
Query is a class which could be only instancied with the
[query static method of model](../model#static-modelquery).

Query is a query builder used to run operation on your database. Like find an instance
or update multiple instances.

Query work directly in function of your schema. Every field declared on your schema
are defined as attribute of the Query. You can split your query into two things :
- The query building part : You choose what your query will do.
- The query run part : You choose the operation to do and execute the query.

## Query building
Query building are in function of the field of your schema. All method are children of
attribute defined in your schema.

!!! example "Little example"
    With this kind of schema :
    ```javascript
    const invoice = new Schema({
        date: Schema.date().required(),
        amount: Schema.number().required(),
    })
    ```
    
    You could use this kind of query :
    ```javascript
    const invoice = await Invoices.query()
        .amount.greaterThan(100) // amount is here in function of your schema
        .findOne();    
    ```

### new Query({ transaction, })
Create a transaction, transaction can not be created directly. You need to use
Model.query().

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| transaction | Transaction | Bind a transaction to the query, you could use Query.transaction for doing the same |

### Query.transaction(transaction)
Bind a transaction to the query.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| transaction | Transaction | Bind a transaction to the query, you could use Query.transaction for doing the same |


### Query.<small style="color:#283593">[field]</small>.is()
Check if the <b style="color:#283593">[field]</b> is equal of the specified value.
```javascript
Query.field.is(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be equal of this value. |

### Query.<small style="color:#283593">[field]</small>.isNot()
Check if the <b style="color:#283593">[field]</b> is not equal of the specified value.
```javascript
Query.field.isNot(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be different of this value. |

### Query.<small style="color:#283593">[field]</small>.isIn()
Check if the <b style="color:#283593">[field]</b> is a member of the parameter array
```javascript
Query.field.isIn(arrayOfValue);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| arrayOfValue | Array.<Mixing\> | The field need to be equal at one array element. |

### Query.<small style="color:#283593">[field]</small>.isNotIn()
Check if the <b style="color:#283593">[field]</b> is not one of the array value.
```javascript
Query.field.isNotIn(arrayOfValue);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| arrayOfValue | Array.<Mixing\> | The field need to be different of each array element. |

### Query.<small style="color:#283593">[field]</small>.greaterThan()
Check if the <b style="color:#283593">[field]</b> is greater than the specified value.
```javascript
Query.field.greaterThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be greater than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).
    
### Query.<small style="color:#283593">[field]</small>.lowerThan()
Check if the <b style="color:#283593">[field]</b> is lower than the specified value.
```javascript
Query.field.lowerThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be lower than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

### Query.<small style="color:#283593">[field]</small>.greaterOrEqualThan()
Check if the <b style="color:#283593">[field]</b> is equal or greater than the 
specified value.
```javascript
Query.field.greaterOrEqualThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be equal or grater than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

### Query.<small style="color:#283593">[field]</small>.lowerOrEqualThan()
Check if the <b style="color:#283593">[field]</b> is equal or lower than
specified value.
```javascript
Query.field.lowerOrEqualThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be equal or lower than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

### Query.<small style="color:#283593">[field]</small>.between()
Check if the <b style="color:#283593">[field]</b> is between min and max.
```javascript
Query.field.between(min, max);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| min | Mixing | The field need to be greater than min. |
| max | Mixing | The field need to be lower than max. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

## Select specific fields
### Query.<small style="color:#283593">[field]</small>.select()
Use to target specific field to be returned by the query.
The <b style="color:#283593">[field]</b> will be part of the output result.
```javascript
Query.field.select();
```

!!!info
    - <b style="color:red;">The json object resulting of the query will not be an instance anymore!</b>
    - If you want to select only one field, you could use selectOnly

### Query.<small style="color:#283593">[field]</small>.selectOnly()
Use to target specific field to be returned by the query.
Only the <b style="color:#283593">[field]</b> will be part of the output result.

```javascript
Query.field.selectOnly();
```

??? example "Example of selectOnly"
    ```javascript
    const email = await User.query()
        ._id.is(TARGET_ID)
        .email.selectOnly()
        .findOne();
    ```

### query.linkedWith()
LinkedWith allow smart query. Ilorm will know the relation between the value and the current Model.
value could be a Query, a [model instance](../model) or a [model Id](../model_id).
```javascript
Query.linkedWith(value)
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Model, Query or Model_ID | the value linked with the given query |

??? example "Example of linkedWith"
    ```javascript
    // Select first unpaid invoice linked with a user with balance > 0
    const QUERY_USER_WITH_POSITIVE_BALANCE = User.query()
        .balance.greaterThan(0);
        
    const invoice = await Invoices.query()
        .isPaid.is(false)
        .linkedWith(QUERY_USER_WITH_POSITIVE_BALANCE)
        .findOne();
    ```


### query.or()
Creating branch.
```javascript
Query.or(orHandler)
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| orHandler | Function | Take a function in parameter used to build subquery |

??? example "Example of or"
    ```javascript
    const email = await User.query()
        .or(orBranch => {
          orBranch().email.is('email');
          orBranch().email.is('alternativeEmail');
        })
        .findOne();
    ```


## Query sorting
### Query.<small style="color:#283593">[field]</small>.useAsSortAsc()
Sorting the result ascending of the query by the given field.
```javascript
Query.field.useAsSortAsc();
```

### Query.<small style="color:#283593">[field]</small>.useAsSortDesc()
Sorting the result descending of the query by the given field.
```javascript
Query.field.useAsSortDesc();
```

## Query pagination
### Query.skip()
Skip a number of element before getting the result.
```javascript
Query.skip(nbToSkip);
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| nbToSkip | Number | The amount of element to skip before starting gathering result. |

### Query.limit()
Limit the number of element impacted by the query.
```javascript
Query.limit(nbToImpact);
```

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| nbToImpact | Number | The amount of element to fetch or update or remove. |


## Query running
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
Update the first element matching the query.

```javascript
// Set isPaid flag to true on the invoice with id INVOICE_ID;
await Invoices.query()
    .id.is(INVOICE_ID)
    .isPaid.set(true)
    .updateOne();
```

### <small style="color:red;">(async)</small> Query.update()
Execute an update targeting all element matching the query.
```javascript
// Change invoices owner
await Invoices.query()
    .ownerId.is(OLD_OWNER)
    .ownerId.set(NEW_OWNER)
    .update();
```

### <small style="color:red;">(async)</small> Query.removeOne()
Remove the first element matching the query.
```javascript
// Remove randomly one invoice;
await Invoices.query()
    .removeOne();
```

### <small style="color:red;">(async)</small> Query.remove()
Remove all elements matching the query
```javascript
// Remove all paid invoices;
await Invoices.query()
    .isPaid.is(true)
    .remove();
```
