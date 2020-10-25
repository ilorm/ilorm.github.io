# Model_Id
A Model ID represent an ID linked with an existing model instance.
You could use a shortcut to work with an instance without loading it
from the database.


### <small style="color:red;">(async)</small> Model.resolveInstance()
Resolve the ids, and load the instance linked with the given id.

```javascript
const modelInstance = await model_id.resolveInstance();
```
