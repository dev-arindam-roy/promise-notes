# Promise Notes
Promise, Promise.all(), Promise.allSettled(), Promise.race(), Promise.any(), Promise.resolve(), Promise.reject()

## Simple Promise - 

```js
function fun1() {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      let isSuccess = false;
      if (isSuccess) {
        console.log('console :: function-1 resolve');
        resolve({key1: 'value-1', key2: 'value-2', key3: 'value-3'});
      } else {
        console.log('console :: function-1 reject');
        reject({error: new Error('error-message-1'), reason: 'error-reject-1'});
      }
    }, 6000);
  });
  return promise;
}
```
```js
console.log(fun1());
/**
Note: 
it will instantly return like [object Promise] { ... }
After 6sec, will print in console - "console :: function-1 resolve"
**/
```
```js
const getFun1 = fun1().then((resolveParam) => console.log(resolveParam));
/**
Note: 
After 6sec, will print in console - "console :: function-1 resolve"
and return the resolve like - 
[object Object] {
  key1: "value-1",
  key2: "value-2",
  key3: "value-3"
}
**/
```

```js
const getFun1 = fun1().catch((rejectParam) => console.log(rejectParam));
/**
Note: 
If i set isSuccess = false, then
After 6sec, will print in console - "console :: function-1 reject"
and return the reject like -
[object Object] {
  error: [object Error] { ... },
  reason: "error-reject-1"
}
**/
```
```js
const getFun1 = fun1().then((resolveParam) => {
  console.log(resolveParam.key1);
  console.log(resolveParam.key2);
  console.log(resolveParam.key3);
});
/**
Note:
Access the passing objects in resolve
**/
```
```js
const getFun1 = fun1().catch((rejectParam) => {
  if (rejectParam.error && rejectParam.error !== undefined) {
    console.log(rejectParam.error.message); // get error object message
  }
  console.log(rejectParam.reason);
});
/**
Note:
If i set isSuccess = false, then
Access the passing objects in reject
**/
```

```js
/** Full Code Like **/

const getFun1 = fun1().then((resolveParam) => {
  console.log(resolveParam.key2);
}).catch((rejectParam) => {
  console.log(rejectParam.error.message);
  console.log(rejectParam.reason);
});

/**
If isSuccess = true then
After 6 sec, will print in console - "console :: function-1 resolve"
and return "value-2"

If isSuccess = false then
After 6 sec, will print in console - "console :: function-1 reject"
and return "error-message-1", "error-reject-1"
**/
```
```js
/** You can write like this **/
const getFun1 = async () => {
  let getResult = await fun1().then((resolveParam) => console.log(resolveParam)).catch((rejectParam) => console.log(rejectParam));
}
/** call it **/
getFun1();
``

```js
/** You can write like this **/
const getFun1 = async () => {
  let getResult = await fun1().then((resolveParam) => {
    return resolveParam;
  }).catch((rejectParam) => {
    return rejectParam;
  });
  return getResult;
}

/** 
As getFun1() again return a promise so to extract result or value from it
we are using then() function here 
**/
getFun1().then((result) => console.log(result));
```
## Example - 1 (Sequential Calls)

```js
let executionTime = 0;

function fun1() {
  let executionTime = 0;
  setInterval(() => { executionTime++ }, 1000);
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      let isSuccess = true;
      if (isSuccess) {
        console.log('console :: function-1 resolve At - ' + executionTime + ' sec');
        resolve({key1: 'value-1', key2: 'value-2', key3: 'value-3'});
      } else {
        console.log('console :: function-1 reject');
        reject({error: new Error('error-message-1'), reason: 'error-reject-1'});
      }
    }, 6000);
  });
  return promise;
}

function fun2() {
  setInterval(() => { executionTime++ }, 1000);
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      let isSuccess = true;
      if (isSuccess) {
        console.log('console :: function-2 resolve At - ' + executionTime + ' sec');
        resolve({key4: 'value-4', key5: 'value-5', key6: 'value-6'});
      } else {
        console.log('console :: function-2 reject');
        reject({error: new Error('error-message-2'), reason: 'error-reject-2'});
      }
    }, 4000);
  });
  return promise;
}

function fun3() {
  setInterval(() => { executionTime++ }, 1000);
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      let isSuccess = true;
      if (isSuccess) {
        console.log('console :: function-3 resolve At - ' + executionTime + ' sec');
        resolve({key7: 'value-7', key8: 'value-8', key9: 'value-9'});
      } else {
        console.log('console :: function-3 reject');
        reject({error: new Error('error-message-3'), reason: 'error-reject-3'});
      }
    }, 2000);
  });
  return promise;
}

fun1().then(async (resolve1) => {
  await fun2().then(async (resolve2) => {
    await fun3().then((resolve3) => {
      console.log(resolve1);
      console.log(resolve2);
      console.log(resolve3);
    }).catch((reject3) => {
      console.log(reject3);
    });
  }).catch((reject2) => {
    console.log(reject2);
  });
}).catch((reject1) => {
  console.log(reject1);
});

/**
Note:
If any promise return failed/reject then we will get error from catch block from that promise
If any promise failed/reject, will get error
If all promises are success then we will get below output.
Output will be like - 
"console :: function-1 resolve At - 6 sec"
"console :: function-2 resolve At - 2 sec"
"console :: function-3 resolve At - 4 sec"
[object Object] {
  key1: "value-1",
  key2: "value-2",
  key3: "value-3"
}
[object Object] {
  key4: "value-4",
  key5: "value-5",
  key6: "value-6"
}
[object Object] {
  key7: "value-7",
  key8: "value-8",
  key9: "value-9"
}
**/
```

## Promise.all

```js
const getResult = Promise.all([fun1(), fun2(), fun3()]).then((successResult) => {
  // get all resolve values in a array 
  console.log(successResult);
}).catch((anyError) => {
  // get only 1 error which will come first  
  console.log(anyError);
});

/**
Note:
It just like the above example with compress code structure
If all promises are success then success result block will execute
If any promise failed/reject then catch block will execute
Catch block get only 1 reject return error respect to which promise return failed/reject
**/
```
## Example - 2 with Promise.all

```js
/** 
getResult is a function which is async and it returns a value that is - returnResult  
returnResult is a collection of responses from fun1, fun2, fun3
**/
const getResult = async () => await Promise.all([fun1(), fun2(), fun3()]).then((successResult) => {
  // get all resolve values in a array
  let returnResult = [];
  if (successResult.length) {
    returnResult.push({
      fun1Resp: successResult[0],
      fun2Resp: successResult[1],
      fun3Resp: successResult[2]
    });
  }
  return returnResult;
}).catch((anyError) => {
  // get only 1 error which will come first  
  console.log(anyError);
});

/**
Now, we call the function getResult() and evaluate the response.
As its return value coming from a promise so we are using then & catch
**/
getResult().then((success) => {
  console.log(success[0].fun1Resp.key1);
  console.log(success[0].fun2Resp.key5);
  console.log(success[0].fun3Resp.key9);
}).catch((error) => {
  console.log(error);
});
```

## Promise.allSettled

```js
const getResult = Promise.allSettled([fun1(), fun2(), fun3()]).then((successResponse) => {
  console.log(successResponse);
}).catch((errorResponse) => {
  console.log(errorResponse);
});

/**
Note:
Promise.allSettled - it execute Sequentialy and grab all response (resolve & reject both)
If any promise resolved then it will return an object with 'status: "fulfilled"' and get all resolve return values within an another key 'value'
Like: - 
[object Object] {
status: "fulfilled",
value: [object Object] {
  key7: "value-7",
  key8: "value-8",
  key9: "value-9"
}
If any promise rejected then then it will return an object with 'status: "rejected"' and get all reject return values within an another key 'reason'
Like: - 
[object Object] {
  reason: [object Object] {
    error: [object Error] { ... },
    reason: "error-reject-1"
  },
  status: "rejected"
}
**/
```

## Example - 2 with Promise.allSettled

```js
/** 
getResult is a function which is async and it returns a value that is - returnResult  
returnResult is a collection of responses from fun1, fun2, fun3
**/
const getResult = async () => await Promise.allSettled([fun1(), fun2(), fun3()]).then((successResult) => {
  // get all resolve values in a array
  let returnResult = [];
  if (successResult.length) {
    returnResult.push({
      fun1Resp: successResult[0],
      fun2Resp: successResult[1],
      fun3Resp: successResult[2]
    });
  }
  return returnResult;
}).catch((anyError) => {
  // get only 1 error which will come first  
  console.log(anyError);
});

/**
Now, we call the function getResult() and evaluate the response.
As its return value coming from a promise so we are using then & catch
**/
getResult().then((success) => {
  console.log(success[0].fun1Resp.status); // "rejected"
  console.log(success[0].fun2Resp.status); // "rejected"
  console.log(success[0].fun3Resp.status); // "fulfilled"
  // your array - object operation here
  // as per your project requirements
}).catch((error) => {
  console.log(error);
});
```

## Promise.any

```js
/**
Note: -
If any promise resolved, it will return that one instantly
It will return 1 success resolve result, respect to which promise return resolve
If all promises are failed/rejected then it will return a error message - "All promises were rejected"
**/
const getResult = Promise.any([fun1(), fun2(), fun3()]).then((successResponse) => {
  console.log(successResponse);
}).catch((errorResponse) => {
  console.log(errorResponse.message);
  // will return "All promises were rejected" if all are rejected
});
```

## Promise.race

```js
/**
Note: - 
It return the first (based on execution time) promise execution instantly
If execution get rejected then will return error/reject values and we will operate it in 'catch' block
If execution get resolved then will return success/resolve values and we will operate it in 'then' block
**/
const getResult = Promise.race([fun1(), fun2(), fun3()]).then((successResponse) => {
  console.log(successResponse);
}).catch((errorResponse) => {
  console.log(errorResponse.error.message);
});
```

## Promise.resolve & Promise.reject

```js
function fun1() {
  return Promise.resolve({key1:'value1', key2:'value2'});
}

function fun2() {
  return Promise.reject({error: new Error('error-message'), key1:'value1', key2:'value2'});
}

console.log(fun1()); // [object Promise] { ... }

/** As fun1 only return resolve so we can use only then block **/
fun1().then((resolveParam) => console.log(resolveParam));
/**
[object Object] {
  key1: "value1",
  key2: "value2"
}
**/

/** As fun2 only return reject so we can use only catch block **/
fun2().catch((rejectParam) => console.log(rejectParam, rejectParam.error.message));
/**
[object Object] {
  error: [object Error] { ... },
  key1: "value1",
  key2: "value2"
}
"error-message"
**/
```
