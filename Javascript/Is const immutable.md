# const는 불변인가?

const 변수에 할당된 값은 재할당을 막기 때문에 바뀌지 않지만 그렇지 않아 보이는 경우가 존재한다.

예를 들어서 const 변수에 Object 를 대입하는 경우에는 const 변수에 주소 값이 할당된다. 그래서 이 주소 값은 불변이지만 Object 에 Key-Value 를 추가하거나 변경하는 행위가 가능하다. 왜냐하면 이 값은 const 에 할당되는 값이 아니기 때문이다.

## Key-Value 불변

ES5에서는 `Object.freeze()` 함수를 통해 Object 의 값을 불변하게 만들 수 있다.

```jsx
const foo = Object.freeze({
	'bar': 27
});
foo.bar = 42; // throws a TypeError exception in stric mode;
							// silently fails in sloppy mode
console.log(foo.bar); // 27
```

하지만 `Object.freeze()` 함수는 얕은 복사(shallow copy)가 일어나서 **Object values** 들이 변경될 수 있다. 이 때는 `deepFreeze()` 구현을 통해 **Object values** 들의 값들을 완전히 불변하게 만들 수 있다.

또한, `Object.freeze()` 함수는 property-value pairs 에만 동작하여, Date, Map or Set 들을 완전히 불변하게 만드는 방법은 없다.

# 요즘 const 사용법

1. 기본적으로 `const` 를 사용한다.
2. 재할당이 필요한 경우에만 `let` 을 사용한다.
3. `var` 는 ES2015에서는 쓰지 않는다.

`let` 이나 `var` 을 사용하였을 경우 사용자가 실수로 값을 변경하거나 한 뒤에 이를 뒤늦게 알아서 추적하려고 자원을 소모하게 되지만 `const` 를 사용하면 코드 정적 분석 단계에서 이를 탐지할 수 있다.

또한 값의 재할당이 필요해 보이지만 `const` 선언 할당 단계에서 이를 해결할 수 있는 방법이 존재하기도 한다.