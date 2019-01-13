# Query
Query is a class which could be only instancied with the
[query static method of model](../model#static-modelquery).

Query is a query builder used to run operation on your database. Like find an instance
or update multiple instance.

Query work directly in function of your schema. Every field declared on your schema
are defined as attribute of the Query. You can split your query into two things :
- The query building part : You choose what your query will do.
- The query run part : You choose the operation to do and execute the query.

### Query building
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

#### Query.<small style="color:#283593">[field]</small>.is()
Check if the <b style="color:#283593">[field]</b> is equal of the specified value.
```javascript
Query.field.is(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be equal of this value. |

#### Query.<small style="color:#283593">[field]</small>.isNot()
Check if the <b style="color:#283593">[field]</b> is not equal of the specified value.
```javascript
Query.field.isNot(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be different of this value. |

#### Query.<small style="color:#283593">[field]</small>.isIn()
Check if the <b style="color:#283593">[field]</b> is a member of the parameter array
```javascript
Query.field.isIn(arrayOfValue);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| arrayOfValue | Array.<Mixing\> | The field need to be equal at one array element. |

#### Query.<small style="color:#283593">[field]</small>.isNotIn()
Check if the <b style="color:#283593">[field]</b> is not one of the array value.
```javascript
Query.field.isNotIn(arrayOfValue);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| arrayOfValue | Array.<Mixing\> | The field need to be different of each array element. |

#### Query.<small style="color:#283593">[field]</small>.greaterThan()
Check if the <b style="color:#283593">[field]</b> is greater than the specified value.
```javascript
Query.field.greaterThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be greater than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).
    
#### Query.<small style="color:#283593">[field]</small>.lowerThan()
Check if the <b style="color:#283593">[field]</b> is lower than the specified value.
```javascript
Query.field.lowerThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be lower than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

#### Query.<small style="color:#283593">[field]</small>.greaterOrEqualThan()
Check if the <b style="color:#283593">[field]</b> is equal or greater than the 
specified value.
```javascript
Query.field.greaterOrEqualThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be equal or grater than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

#### Query.<small style="color:#283593">[field]</small>.lowerOrEqualThan()
Check if the <b style="color:#283593">[field]</b> is equal or lower than
specified value.
```javascript
Query.field.lowerOrEqualThan(value);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| value | Mixing | The field need to be equal or lower than value. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

#### Query.<small style="color:#283593">[field]</small>.between()
Check if the <b style="color:#283593">[field]</b> is between min and max.
```javascript
Query.field.between(min, max);
```
Returns a query to chain call.

| Parameter        | Type    | Description              |
|:----------------:|:-------:| ------------------------ |
| min | Mixing | The field need to be greater than min. |
| max | Mixing | The field need to be lower than max. |

!!! info
    Only work with SchemaField/Number or SchemaField/Date (or with some plugin SchemaField).

### Query running
#### <small style="color:red;">(async)</small> Query.findOne()
Execute the query, and returns one element which match it.

```javascript
const instance = await query.findOne();
```
Return one instance which match the query.

#### <small style="color:red;">(async)</small> Query.find()
Execute the query, and returns one array containing all elements which match the query.

```javascript
const arrayOfInstances = await query.find();
```
Return all instance which match the query.

#### <small style="color:red;">(async)</small> Query.stream()
Execute the query, and returns the stream of all elements matching the query

```javascript
const streamOfInstances = await query.stream();
```
Return a stream containing all elements matching the query.

#### <small style="color:red;">(async)</small> Query.count()
Execute the query, and return how many item match the query.

```javascript
const numberOfInstance = await query.count();
```
Return the number of instance which match the query.

#### <small style="color:red;">(async)</small> Query.updateOne()

#### <small style="color:red;">(async)</small> Query.update()

#### <small style="color:red;">(async)</small> Query.removeOne()

#### <small style="color:red;">(async)</small> Query.remove()
