# Short Circuit Evaluation이란?

Short Circuit Evaluation이란 `AND` 혹은 `OR` 의 연산에 있어서 결과가 확실하게 예측이 되었을 때 뒤에 나머지 연산을 실행하지 않고 답을 내버리는 경우를 의미한다.

## AND 연산의 경우

- `AND` 연산의 경우에 `false` 가 우선 나와버리면 `AND` 뒤에 나오는 연산은 생략된다.

## OR 연산의 경우

- `OR` 연산의 경우에 `true` 가 우선 나와버리면 `OR` 뒤에 나오는 연산은 생략된다.

### 예제

```jsx
function foo() {
	return true;
}

false && foo() // false 는 어떤값과 && 연산을 해도 false, 즉 foo()를 실행하지 않는다.
false || foo() // false 는 true 와 || 연산을 하면 true, 즉 foo()를 실행한다.
```