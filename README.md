# Promise Notes
Promise, Promise.all(), Promise.allSettled(), Promise.race(), Promise.any(), Promise.resolve(), Promise.reject()

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
