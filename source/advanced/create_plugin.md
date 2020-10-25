# Create a plugin
Ilorm allows you to create plugin to add feature to the ORM. 

## Understand the internal
Ilorm use class, a plugin need to overload one or more class. 

For example, a Model is always composed of at least two class;
- BaseModel: The lowest representation of a Model, 100% internal, you could overload.
- InternalModel: The final model which inherits from BaseModel (or plugin).

```javascript
class InternalModel extends BaseModel {}
```

Adding a plugin, overload Base Model, and your plugin class is passed to InternalModel;
```javascript
class YourPluginModel extends BaseModel {}
class InternalModel extends YourPluginModel {}
```

Using more than one plugin, overloading Base Model, make chainable inheritance;
```javascript
class YourPluginModel1 extends BaseModel {}
class YourPluginModel2 extends YourPluginModel1 {}
class InternalModel extends YourPluginModel2 {}
```

To make this magic happen, your plugin need to be a class factory. The factory will take as parameter the "parent"
Model;
```javascript
const yourModelFactory = ({ ParentModel, }) => {
  return class YourPluginModel extends ParentModel {

  };
};
```

To make this factory usable by ilorm, you need to follow an interface; Ilorm will check if factory exists in the plugin
and use the factory if it's declared;
```javascript
ilorm.use({
  core: {
    modelFactory: yourModelFactory,
  }
})
```

## Interface
Ilorm let you overload everything, a plugin need to follow this interface;
```javascript
{
  core: {
    modelFactory,
    modelIdFactory,
    queryFactory,
    schemaFactory,
    baseFieldFactory,
    fieldFactories: {
      booleanFieldFactory,
      dateFieldFactory,
      numberFieldFactory,
      stringFieldFactory,
      ...
      anyCustomFieldFactory,
    },
    transactionFactory,
  }
}
```

Every factory take as parameter the parent class, and need to return the overloaded class a result.

fieldFactories is a bit different. It's allow you to create your own field. You could use it to overload existing field
or create new one.

## Example: Build a plugin to add soft delete
In this tutorial we will create a plugin to allow "soft" delete.

### New field isDeleted in all schema
First we will create a isDeleted field into all schema, to do it, we will overload the default Schema;

```javascript
function SchemaFactory(Schema) {
  return class SoftDeleteSchema extends Schema {
    constructor(schema) {
      schema.isDeleted = this.constructor.boolean()
        .required().default(false);

      super(schema);
    }
  }
}
```

### Change behavior of query to ignore deleted instance by default
We will add a constraint to ignore deleted instance when creating a query

```javascript
function QueryFactory(Query) {
  return class SoftDeleteQuery extends Query {
    constructor(...params) {
      const query = super(...params);

      query.isDeleted.is(false);

      return query;
    }
  }
}
```

### Change behavior of delete
Soft delete means, updating the flag isDeleted to true.

```javascript
function QueryFactory(Query) {
  return class SoftDeleteQuery extends Query {
    // ...

    remove(...params) {
      this.isDeleted.set(true);

      return this.update(...params);
    }

    removeOne(...params) {
      this.isDeleted.set(true);

      return this.update(...params);
    }
  }
}
```

### Had hard delete, if we want to delete instance
Let a way to the user to force the deletion of an instance.

```javascript
function QueryFactory(Query) {
  return class SoftDeleteQuery extends Query {
    // ...

    hardRemove(...params) {
      return super.remove(...params);
    }

    hardRemoveOne(...params) {
      return super.removeOne(...params);
    }
  }
}
```

### Wrap up
```javascript
function SchemaFactory(Schema) {
  return class SoftDeleteSchema extends Schema {
    constructor(schema) {
      schema.isDeleted = this.constructor.boolean()
        .required().default(false);

      super(schema);
    }
  }
}

function QueryFactory(Query) {
  return class SoftDeleteQuery extends Query {
    constructor(...params) {
      const query = super(...params);

      query.isDeleted.is(false);

      return query;
    }

    remove(...params) {
      this.isDeleted.set(true);

      return this.update(...params);
    }

    removeOne(...params) {
      this.isDeleted.set(true);

      return this.update(...params);
    }

    hardRemove(...params) {
      return super.remove(...params);
    }

    hardRemoveOne(...params) {
      return super.removeOne(...params);
    }
  }
}

module.exports = {
  core: {
    queryFactory: QueryFactory,
    schemaFactory: SchemaFactory,
  },
};
```
