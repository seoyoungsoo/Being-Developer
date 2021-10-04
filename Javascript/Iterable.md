반복 가능한 객체(iterable object)는 `for...of` 구문과 함께 ES2015에서 도입되었다.

반복 가능한 객체를 다른 객체와 구분짓는 특징은, 객체의 `Symbol.iterator` 속성에 **특별한 형태의 함수**가 들어있다는 것이다.

```jsx
const str = 'hello';
str[Symbol.iterator]; // [Function]
```

객체의 `Symbol.iterator` 속성에 특정 형태의 함수가 들어있다면, 이를 반복 가능한 객체(iterable object) 혹은 줄여서 **iterable**이라 부르고, 해당 객체는 **Iterable protocol을 만족한다**고 말한다.

내장된 생성자 중 iterable 객체를 만들어내는 생성자에는 아래와 같은 것들이 있다.

- `String`
- `Array`
- `TypedArray`
- `Map`
- `Set`

# Iterable의 사용

어떤 객체가 Iterable이라면, 그 객체에 대해서 아래의 기능들을 사용할 수 있다.

- `for...of` 루프
- spread 연산자 (`...`)
- 분해대입(destructuring assignment)
- 기타 iterable을 인수로 받는 함수

즉, **문자열에 대해서도 위 기능들을 사용할 수 있다.**

```jsx
// `for...of`
for (let c of 'hello') {
	console.log(c);
}

// spread 연산자
const characters = [...'hello'];

// 분해대입
const [c1, c2] = 'hello';

// `Array.from`은 iterable 혹은 array-like 객체를 인수로 받는다.
Array.from('hello');
```

# Generator 함수

Iterable protocol을 구현하기만 한다면 **어떤 객체든 iterable이 될 수 있다.**

Iterable을 구현하는 가장 쉬운 방법은 ES2015에 도입된 **generator 함수**를 사용하는 것이다.

Generator 함수는 **iterable 객체를 반환하는 특별한 형태의 함수**이다.

```jsx
// generator 함수 선언하기
function* gen1() {
	// ...
}

// 표현식으로 사용하기
const gen2 = function* () {
	// ...
}

// 메소드 문법으로 사용하기
const obj = {
	* gen3() {
		// ...
	}
}
```

Generator 함수를 호출하면 객체가 생성되는데, 이 객체는 iterable protocol을 만족한다.

즉, `Symbol.iterator` 속성을 갖고 있다.

```jsx
function* gen1() {
	// ...
}

// `gen1`를 호출하면 iterable이 반환된다.
const iterable = gen1();

iterable[Symbol.iterator]; // Function
```

Generator 함수 안에서는 `yield` 라는 특별한 키워드를 사용할 수 있다.

Generator 함수 안에서 `yield` 키워드는 `return` 과 유사한 역할을 하며, iterable의 기능을 사용할 때 `yield` 키워드 뒤에 있는 값들을 순서대로 넘겨준다.

```jsx
function* numberGen() {
	yield 1;
	yield 2;
	yield 3;
}

// 1, 2, 3이 순서대로 출력된다.
for (let n of numberGen()) {
	console.log(n);
}
```

`yield*` 표현식을 사용하면, 다른 generator 함수에서 넘겨준 값을 대신 넘겨줄 수도 있다.

```jsx
function* numberGen() {
	yield 1;
	yield 2;
	yield 3;
}

function* numberGen2() {
	yield* numberGen();
	yield* numberGen();
}

// 1, 2, 3, 1, 2, 3이 순서대로 출력된다.
for (let n of numberGen2()) {
	console.log(n);
}
```

`yield` 키워드를 제외하면, generator 함수 내부의 동작 방식은 일반적인 함수와 별반 다르지 않다. 즉, 다른 함수에서 할 수 있는 일이라면 generator 함수 안에서도 모두 할 수 있다.

```jsx
// 등차수열 생성하기
function* range(start = 0, end = Infinity, step = 1) {
	for (let i = start; i < end; i += step) {
		yield i;
	}
}

// 피보나치 수열 생성하기
function* fibonacci(count = Infinity) {
	let x = 1, y = 1;
	for (let i = 0; i < count; i++) {
		yield x;
		[x, y] = [y, x + y];
	}
}

// 하나의 항목을 계속 넘겨주기
function* repeat(item, count = Infinity) {
	for (let i = 0; i < count; i++) {
		yield item;
	}
}

// 여러 요소를 반복해서 넘겨주기
function* repeatMany(array) {
	while (true) {
		for (let item of array) {
			yield item;
		}
	}
}
```

## Generator 함수 사용 주의점

- Generator 함수로부터 생성된 Iterable은 한 번만 사용할 수 있다.
- Generator 함수 내부에서 정의된 일반 함수에서는 `yield` 키워드를 사용할 수 없다.

```jsx
// Generator 함수로부터 생성된 iterable은 한 번만 사용될 수 있다.
function* gen() {
	yield 1;
	yield 2;
	yield 3;
}

const iter = gen();

for (let n of iter) {
	// 잘 출력됨
	console.log(n);
}

for (let n of iter) {
	// `iter`는 한 번 사용되었으므로, 이 코드는 실행되지 않는다.
	console.log(n);
}
```

```jsx
// Generator 함수 내부에서 정의된 일반 함수에서는 `yield` 키워드를 사용할 수 없다.
function* gen2() {
	// 문법 오류 발생 (Unexpected token)
	function fakeGen() {
		yield 1;
		yield 2;
		yield 3;
	}
	fakeGen();
}
```

# Iterator Protocol

이제 iterable의 동작 원리를 살펴보자.

앞에서 'iterable 객체는 iterable protocol을 만족한다. 즉, `Symoble.iterator` 속성에 **특별한 형태의 함수가 저장되어 있다**고 했다.

Iterable protocol을 만족하려면, `Symbol.iterator` 속성에 저장되어 있는 함수는 **iterator** 객체를 반환해야 한다.

Iterator 객체는 아래의 특별한 조건을 만족하는 객체이다.

- Iterator는 `next` 라는 메서드를 갖는다.
- `next` 메서드는 다음 두 속성을 갖는 객체를 반환해야 한다.
    - `done` - 반복이 모두 끝났는지를 나타낸다.
    - `value` - 현재 순서의 값을 나타낸다.

위 조건을 **iterator protocol**이라고 한다.

```jsx
// 문자열은 iterable이므로 이로부터 iterator를 생성할 수 있다.
const strIterator = 'go'[Symbol.iterator]();
strIterator.next(); // { value: 'g', done: false }
strIterator.next(); // { value: 'o', done: false }
strIterator.next(); // { value: undefined, done: true }
strIterator.next(); // { value: undefined, done: true }

// generator 함수로부터 생성된 객체 역시 iterable이므로 이로부터 iterator를 생성할 수 있다.
function* gen() {
	yield 1;
	yield 2;
}

const genIterator = gen()[Symbol.iterator]();
genIterator.next(); // { value : 1, done: false }
genIterator.next(); // { value : 2, done: false }
genIterator.next(); // { value: undefined, done: true }
genIterator.next(); // { value: undefined, done: true }
```

직접 iterator을 만들어보자

```jsx
function range(start = 0, end = Infinity, step = 1) {
	// `range` 함수는 iterable을 반환한다.
	return {
		currentValue: start,
		[Symbol.iterator]() {
			// iterable의 `Symbol.iterator` 메서드는 iterator를 반환해야 한다.
			return {
				next: () => {
					if (this.currentValue < end) {
						const value = this.currentValue;
						this.currentValue += step;
						return {
							done: false,
							value
						}
					} else {
						return {
							done: true
						}
					}
				}
			};
		}
	}
}
```

Generator 함수를 사용했을 때보다 훨씬 복잡해진다. 이러한 이유 때문에 iterator protocol을 직접 구현하는 대신 generator 함수를 사용하는 경우가 많지만, `next` 메소드를 사용하면 iterable을 세부적으로 제어할 수 있으므로, iterator에 대해서 알아두자!

# Generator와 Iterator

Generator 함수로부터 만들어진 객체는 일반적인 iterable처럼 쓸 수 있지만, iterator와 관련된 특별한 성질을 갖고 있다.

첫 번째로, generator 함수로부터 만들어진 객체는 **iterable protocol과 iterator protocol을 동시에 만족한다.**

```jsx
function* gen() {
	// ...
}

const genObj = gen();
genObj[Symbol.iterator]().next === genObj.next; // true
```

즉, `Symbol.iterator` 를 통해 iterator를 생성하지 않고도 바로 `next` 를 호출할 수 있다.

```jsx
function* gen() {
	yield 1;
	yield 2;
	yield 3;
}

const iter = gen();

iter.next(); // { value: 1, done: false }
iter.next(); // { value: 2, done: false }
iter.next(); // { value: 3, done: false }
iter.next(); // { value: undefined, done: true }
```

두 번째로, generator 함수 안에서 `return` 키워드를 사용하면 반복이 바로 끝나면서 `next` 메소드에서 반환되는 객체의 속성에 앞의 반환값이 저장된다. 다만, `return` 을 통해 반환된 값이 반복 절차에 포함되지는 않는다.

```jsx
function* gen() {
	yield 1;
	return 2; // genrator 함수는 여기서 종료된다.
	yield 3;
}

const iter = gen();

iter.next(); // { value: 1, done: false }
iter.next(); // { value: 2, done: true }
iter.next(); // { value: undefined, done: true }

// `1`만 출력된다.
for (let v of gen()) {
	console.log(v);
}
```

세 번째로, generator 함수로부터 생성된 객체의 `next` 메서드에 인수를 주어서 호출하면, generator 함수가 멈췄던 부분의 `yield` 표현식의 결과값은 앞에서 받은 인수가 된다.

```jsx
function* gen() {
	const received = yield 1;
	console.log(received);
}

const iter = gen();
iter.next(); // { value: 1, done: false }

// 'hello'가 출력된다.
iter.next('hello'); // { value: undefined, done: true}
```

Generator 함수의 이런 성질은 비동기 프로그래밍을 위해 활용되기도 한다. 자세한 내용은 다음에 알아보자.