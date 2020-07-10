# Create a connector

## The connector class
### Basics
To create a connector, you need to implement a class Connector.

The class Connector is always linked with a database
The instance of the class is linked with a table, a collection or a subset of data.

Example :

* On a SQL database

      * The class is linked with the full database
      
      * An instance of the class is linked with a table
      
* On a MongoDB database

      * The class linked with the full database
      
      * An instance of the class is linked with a collection
      

### Mandatory operation
The class need to implement mandatory operation to be fully functional.
The operations are divided into :

* Database related : Update, Find, Create, Remove data

* Connector factory : The connector instantiate his own Query class or Model class to overwrite
basic behavior.


### Database related
#### Ilorm query
The majority of database related operation receive a Query instance as parameter.
You need to use this query instance to create the same query in the database language
to do it. The query have some useful method : queryBuilder and updateBuilder.

##### queryBuilder
query.queryBuilder is use to restrict your query. You will put as parameter of queryBuilder
a set of handler. Each handler should be called back (in function of the query).
```javascript
query.queryBuilder({
  // On option will be called if skip & limit was put :
  onOption: (skip, limit) => {},
  
  // If an or operator is used on the query, this handler will be called
  // The parameter is multiple query instance (each or branch)
  onOr: arrayOfQuery => {},
  
  // If one or more field are select, this handler will be called
  // You need to restrict result only to the specified field
  onSelect: ({ field, }) => {},
  
  // Called (in order), to set the order of the result.
  // Key is the target sorting key
  // behavior precise if it's ascending or descending sorting
  onSort: ({ key, behavior, }) => {},
  
  // Called per each query operation
  // Key is the target key
  // Operation is EQUAL, NOT_EQUAL...
  // Value is the value applied to the operation
  onOperator: ({ key, operation, value, }) => {}
});
```
Behavior and operation are equal to specific constants. The constants can be found in
`ilorm-constants` in the QUERY constants :
```javascript
const { QUERY, } = require('ilorm-constants');

// Possible value of operation on the onOperator handler :
const { OPERATIONS } = QUERY;
OPERATION.IS // equal (===)
OPERATION.IS_NOT // Not equal (!==)
OPERATION.IS_IN // The value is in the specified array of possibility
OPERATION.IS_NOT_IN // The value is not in the specified array of possibility
OPERATION.GREATHER_THAN // Superior of the value
OPERATION.LOWER_THAN // Inferior of the value
OPERATION_GREATHER_OR_EQUAL_THAN // Superior or equal of the value
OPERATION.LOWER_OR_EQUAL_THAN // Inferior or equal of the value

// Possible value of behavior on the onSort handler :
const { SORT_BEHAVIOR } = QUERY;
SORT_BEHAVIOR.ASCENDING // Ascending order
SORT_BEHAVIOR.DESCENDING // Descending order
```

##### updateBuilder
query.updateBuilder is used to handle the update kind of query. Like the queryBuilder
you need to give him handler to build your query
```javascript
query.updateBuilder({
  // Apply an update operation
  // Key is the targeted key
  // Operator is the operation to make
  // Value is the value to apply
  onOperator: ({ key, operator, value, }) => {},
})
```

You can use the OPERATIONS constant to compare with the operator
```javascript
const { OPERATIONS } = require('ilorm-constants').QUERY;

// Set the value associated with the key :
OPERATIONS.SET

// Increment the value associated with the key (could be a negative number to decrement) :
OPERATIONS.ADD
```

#### Database operation to implement
This part list all database related method to implement in your Connector.
All operations are async :

* count(query) : Return the number of element associated with the specified query.

* create(item|arrayOfItem) : The handler need to create all item into the database.

* find(query) : Return an array of all element linked with the specified query.

* findOne(query) : Return the first element linked with the specified query.

* remove(query) : Remove all element which match the query

* removeOne(query) : RemoveOne element which match the query

* stream(query) : Return a stream which stream all element associated with the query.

* update(query) : Update all element which match the query

* updateOne(query) : Update the first element which match the query

### Connector factory
On your connector, you need to implement to more method :

* modelFactory : Will return a Model class linked with your connector.

* queryFactory : Will return a Query class linked with your connector.

Both method take in parameter a base Class used as parent of the future class.

#### modelFactory
With ilorm, every Model are inheritance of two class minimum :

* BaseModel : The lowest level, this class is defined in ilorm core.

* FactoryModel : Overload BaseModel to specify name, schema, connector, plugin, linked with 
the current model. This Class is generated to every model when calling ilorm.newModel. This
class is defined in ilorm core too. This class is given in parameter of ModelFactory

* DatabaseModel : The last mandatory level, it's the result the modelFactory, you can add
every kind of specific behavior of your database here.


* Plugin could overload BaseModel (you can have Model inheritance between BaseModel and
FactoryModel).
* The final user could overload your DatabaseModel.

The signature of modelFactory is ;
DatabaseModel result = Connector.modelFactory({ name, schema, ParentModel });

* name : The name of the model, given by the developer who using your connector.

* schema : The schema binded with the current model.

* ParentModel : The FactoryModel (Your Model need to inherit from this parameter).

#### queryFactory
Query are similar to Model. Every query are composed by a BaseQuery (defined in core),
overloaded by InternalQuery to specify the binded model. And your connector Query is the last
part of the system. Plugin could overload BaseQuery.

The signature of the queryFactory is :
DatabaseQuery result = Connector.queryFactory({ schema, ParentQuery, });

* schema : The schema binded with your query

* ParentQuery : The ParentQuery (your Query need to inherit from this parameter)
