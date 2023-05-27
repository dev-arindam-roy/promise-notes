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
        reject({error: new Error('error-message-from-function1'), reason: 'error-reject-1'});
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
