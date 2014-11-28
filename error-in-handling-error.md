
# bugs within error handlers
When handling error throws some errors, it woull be sillently dismissed and there is no way of knowing what's wrong.
```
var Q = require('q');
function asyncTask() {
    return Q.reject();
}

function errorHandler() {
    // calling log of undefined logger, throws error
    logger.log();
}

/** sample async task which would throw error */
asyncTask()
.fail(function() {
    /** handling error with a bug in itself */
    errorHandler();
})
```

To fix it, we should call `.done()` at the end of promise chain, so if there are any errors not catched yet, they will be thrown.
```
asyncTask()
.fail(function() {
    errorHandler();
})
.done()
```
