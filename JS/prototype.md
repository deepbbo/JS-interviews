# 프로토타입

<br>

### [프로퍼티가 뭔가요?]

프로토타입은 자바스크립트에서 객체지향 프로그래밍의 상속을 구현하기 위해 사용되는 객체입니다.

모든 객체는 프로토타입을 가지고 있고 이는 생성자 함수와 연결되어 있습니다.

객체를 통해 어떤 프로퍼티나 메서드를 참조하려고 할 때 1차적으로 객체 본인에서 프로퍼티와 메서드를 찾고
본인에게 없다면 프로토타입 체인을 통해 객체 본인의 프로토타입(상위 객체)에서 프로퍼티와 메서드를 찾습니다.
이를 통해 객체지향의 상속을 구현할 수 있습니다.

<br><br>

> ## \- 꼬리질문 -

<br>

### [프로토타입에는 어떻게 접근하나요?]

모든 객체는 [[Prototype]]을 지니고 있는데 **proto** 접근자 프로퍼티로 프로토타입에 간접 접근할 수 있습니다.

[__proto__ 접근자 프로퍼티는 객체 본인에게 있는 프로퍼티 인가요?]

아닙니다. **proto** 접근자 프로퍼티는 모든 객체의 프로토타입 객체인 Object의 접근자 프로토타입입니다.

```jsx
console.log({}.__proto__ === Object.prototype); // true
```

<br><br>

### [프로토타입은 언제 생성되나요?]

프로토타입은 생성자 함수가 생성되는 시점에 같이 생성됩니다.

<br><br>

### [사용예시 간단하게 들어보실 수 있나요?]

네, 예를 들어 name, age라는 프로퍼티를 갖는 Person 클래스가 있습니다.

이때 Person.prototype.sum = (a, b) ⇒ a + b; 라고 정의했을 때
sum이라는 함수는 Person클래스에 생기지 않고 Person의 프로토타입에 정의됩니다.

그리고 Person의 인스턴스를 생성하면 인스턴스의 프로토타입은 Person의 프로토타입을 가리키게 됩니다.

이 인스턴스는 sum이라는 함수를 쓸 수 있게 되는데, 이때 sum이라는 함수는 1차적으로 인스턴스 본인에게서 찾고,
없으면 인스턴스의 프로토타입인 Person의 프로토타입에 접근해서 sum이라는 함수를 참조합니다.

```jsx
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

Person.prototype.sum = (a, b) => a + b;

const me = new Person("me", "24");

console.log(me.sum(5, 10));                    // 15
console.log(me.sum === me.__proto__.sum);      // true
console.log(me.sum === Person.prototype.sum);  // true
```
