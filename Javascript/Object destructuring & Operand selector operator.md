# Object destructuring 이란?

디스트럭처링(Destructuring)은 구조화된 배열 또는 객체를 Destructuring(비구조화, 파괴)하여 개별적인 변수에 할당하는 것이다.

배열 또는 객체 리터럴에서 필요한 값만을 추출하여 변수에 할당하거나 반환할 때 유용하다.

ES6의 객체 디스트럭처링은 객체의 각 프로퍼티를 객체로부터 추출하여 변수 리스트에 할당한다. 이 때, 할당 기준은 **프로퍼티 이름(키)**이다.

```jsx
// ES6 Destructuring

const obj = { firstName: 'John', lastName: 'Doe' };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.

// 변수 lastName, firstName가 선언되고
// obj(initializer(초기화자))가 Destructuring(비구조화, 파괴)되어 할당된다.

const { firstName, lastName } = obj;

console.log(firstName, lastName); // John Doe
```

만약 firstName 이 undefined가 올 가능성이 있어서 이를 초기화 하고 싶을 경우 아래와 같이 backup value로 초기화 할 수 있다.

```jsx
const obj = { firstName: undefined, lastName: 'Doe' };
const { firstName = 'David', lastName } = obj;

console.log(firstName, lastName); // David Doe
```

하지만 이는 ES6의 destructuring default values 가 undefined일 경우에만 초기화가 가능하고 자바스크립트에서 falsy value인 `null, false, '', 0` 등에는 해당하지 않는다.

```jsx
const obj = { firstName: '', lastName: 'Doe' };
const { firstName = 'David', lastName } = obj;

console.log(firstName, lastName); // Doe
```

위의 firstName이 `''` 만이 아닌 falsy value에 해당하는 경우에 원하는 값으로 초기화 하고 싶을 경우 아래와 같이 값을 초기화 할 수 있다.

```jsx
const obj = { firstName: '', lastName: 'Doe' };
const { lastName } = obj;
const fistName = obj.firstName || 'David';

console.log(firstName, lastName); // David Doe
```

위의 `||` 연산자는 단순히 boolean checker 로 동작하는 것이 아닌 변수의 값이 존재하지 않거나 falsy value 로 판별될 경우 `||` 뒤의 backup default value (`'David'`)로 초기화 시켜주는 **Operand selector operator** 의 역할을 한다.