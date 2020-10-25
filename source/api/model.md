# Model
Model could not be created directly. To access this API, you need to invoke the
[newModel](../core#ilormnewmodel) method from ilorm. And create your own child model.

### <small style="color:blue;">(static)</small> Model.query()
Create a [Query](../query) instance targeting the current Model.

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

### <small style="color:blue;">(static)</small><small style="color:red;">(async)</small> Model.getById()
Get an instance of the model by it's Id.
```javascript
const modelInstance = await Model.getById(id);
```
Return the instance associated with the given id.

| Parameter        | Type    | Default | Description              |
|:----------------:|:-------:|:-------:| ------------------------ |
| id | Mixing |  | The id of the instance to get. |

### <small style="color:blue;">(static)</small> Model.id(ids)
Create a [model Id](../model_id) linked with the ids parameter.

| Parameter        | Type    | Default | Description              |
|:----------------:|:-------:|:-------:| ------------------------ |
| ids | Mixing |  | Create a Model_Id linked with ids |

### <small style="color:red;">(async)</small> Model.save()
Save the current instance into the database.
- If the instance exists, it will make an update with the updated field only.
- If the instance does not exists. It will create the instance into the database.

```javascript
await modelInstance.save();
```

### <small style="color:red;">(async)</small> Model.remove()
Remove the current instance from the database.

```javascript
await modelInstance.remove();
```
