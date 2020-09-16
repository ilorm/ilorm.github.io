# Transaction
Ilorm support transaction with the Transaction class.

```javascript
const { Transaction } = require('ilorm');
```

You could use Transaction object directly;
```javascript
const transaction = new Transaction();

try {
  Model.query({ transaction })
    // do something
  await transaction.commit();
} catch(err) {
  await transaction.rollback();
}
```
Or use the Transaction.run method;
```javascript
Transaction.run(async (transaction) => {
  Model.query({ transaction })
    // do something
})
```



### <small style="color:blue;">(static)</small> <small style="color:red;">(async)</small> Transaction.run(hander)
Create a context of transaction and run the handler.
If an exception is throw, the transaction is rollback.
If the run is done without exception, the transaction is commit.

```javascript
// If node support async local storage (Node 14 and above);
Transaction.run(async () => {
  // Every ilorm stuff done here, are managed by the transaction;
  const invoice = await Invoice.query()
    .id.is(invoiceId)
    .findOne();

  const userAcount = await Account.query()
    .userId.is(invoice.userId)
    .findOne();
 
  if (userAccount.balance >= invoice.amount) {
    invoice.isPaid = true;
    userAccount.balance -= invoice.amount;
    await Promise.all([
      invoice.save(),
      userAccount.save(),
    ]);
  }
});

// If node does not support local storage
Transaction.run(async (transaction) => {
  // Every ilorm stuff done here, are managed by the transaction;
  const invoice = await Invoice.query({ transaction }) // First way to bind a transaction to a query
    .id.is(invoiceId)
    .findOne();

  const userAcount = await Account.query()
    .transaction(transaction) // Alternative way to bind a transaction to a query
    .userId.is(invoice.userId)
    .findOne();
 
  if (userAccount.balance >= invoice.amount) {
    invoice.isPaid = true;
    userAccount.balance -= invoice.amount;
    await Promise.all([
      invoice.save({ transaction }),
      userAccount.save({ transaction }),
    ]);
  }
});
```

### <small style="color:red;">(async)</small> Transaction.commit();
Apply transaction

### <small style="color:red;">(async)</small> Transaction.rollback();
Rollback transaction
