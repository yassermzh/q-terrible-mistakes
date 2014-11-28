
# done()
When the error handler in fail throw some errors, it woull be sillently dismissed and there is no way of knowing what's wrong.
```
var Q = require('q');
function asyncTask() {
    return Q.reject();
}

function errorHandler() {
    // calling log of undefined logger, throws error
    logger.log();
}

asyncTask()
.fail(function() {
    errorHandler();
})
```

To fix it, we should call `.done()` at the end of promise chain, so if there are any errors not catched yet, they you be thrown.
```
asyncTask()
.fail(function() {
    errorHandler();
})
.done()
```
