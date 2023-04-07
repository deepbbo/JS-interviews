# 비동기 처리에 대한 설명(Callback, Promise, async await)

비동기 처리란 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 것을 말한다.

> ## Callback
콜백 함수로 비동기 처리를 할 수 있다.

```javascript
console.log("Hello");
setTimeout(function () {
  console.log("Bye");
}, 3000);
console.log("Hello Again");
```

콜백 함수를 익명함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 늘어남에 따라서 콜백 지옥 현상이 발생하는 문제점이 있다.

```js
setTimeout(
  function (name) {
    let coffeeList = name;
    console.log(name);

    setTimeout(
      function (name) {
        coffeeList += ", " + name;
        console.log(name);

        setTimeout(
          function (name) {
            coffeeList += ", " + name;
            console.log(name);

            setTimeout(
              function (name) {
                coffeeList += ", " + name;
                console.log(name);
              },
              500,
              "카페라떼"
            );
          },
          500,
          "카페모카"
        );
      },
      500,
      "아메리카노"
    );
  },
  500,
  "에스프레소"
);
// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 에스프레소, 아메리카노, 카페모카, 카페라떼
```
이를 해결하기 위해서 ES6에서 Promise가 도입되었다.


> ## Promise

```js
new Promise(executor);
```
Promise 객체는 new 키워드와 생성자를 사용해서 만든다. 생성자는 매개변수로 executor라는 콜백 함수를 받는데 이 콜백 함수는 인자로 resolve, reject함수를 인자로 받는다.

```js
const myFirstPromise = new Promise((resolve, reject) => {
  // do something asynchronous which eventually calls either:
  //
  //   resolve(someValue)        // fulfilled
  // or
  //   reject("failure reason")  // rejected
});
```
resolve 함수는 비동기 작업을 성공적으로 완료해 결과를 값으로 반환할 때 호출하고
reject 함수는 작업이 실패하여 오류의 원인을 반환할 때 호출하면 된다.

resolve, reject 의 처리 결과 모두 후속 처리 메소드로 전달한다.

후속 처리 메소드
Promise 객체의 상태에 따라 후속 처리 메소드를 호출하게 된다.

```js
promise
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log("finally");
  });
  ```
- then() 메소드는 두 개의 콜백 함수를 인자로 받는다.
  
  ```js
  .then(onFulfilled, onRejected);
  ```
첫 번째 콜백 함수는 resolve함수가 호출되면 호출되고, 두 번재 콜백 함수는 reject함수가 호출되면 호출된다.

- catch() 메소드는 예외(비동기 처리 또는 then 메소드에서 발생한 에러)가 발생하면 호출된다.
  ```js
  .catch(onRejected);
  ```

- finally() 메소드는 Promise가 처리되면 상태에 상관없이 지정된 콜백 함수가 실행된다.
  ```js
  .finally(onFinally);
  ```

에러 처리는 then의 두 번째 콜백 함수 또는 catch로 에러 핸들링이 가능하지만 catch로 하는 것이 더 좋다.

그 이유는 then으로 에러 핸들링을 하게되면 then의 첫 번째 콜백 함수에서 발생하는 에러는 잘 잡아내지 못하기 때문이다.

```
출처
https://velog.io/@tlatjdgh3778/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90-%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91-%EC%A7%88%EB%AC%B8-%EC%A0%95%EB%A6%AC1#%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EC%84%A4%EB%AA%85callback-promise-async-await
```

https://velog.io/@cjh951114/%EB%8B%A8%EA%B3%A8%EC%A7%88%EB%AC%B8-04.-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EC%84%A4%EB%AA%85
ㄴ https://velog.io/@cjh951114?tag=%EB%A9%B4%EC%A0%91