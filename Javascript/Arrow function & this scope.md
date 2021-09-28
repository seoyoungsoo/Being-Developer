# Arrow function

구문이 짧고 항상 익명이며, 메소드 함수가 아닌 곳에 가장 적합하다. 그래서 생성자로 사용할 수 없다.

## 메서드 함수에서 부적합!

```jsx
const obj = {
	num: 10,
	arrowFunc: () => console.log(this.num, this),
	nomalFunc: function() {
		console.log(this.num, this);
	}
}

obj.arrowFunc(); // undefined {}
obj.nomalFunc(); // 10 { num: 10, arrowFunc ... }
```

### 메서드(method) vs 함수(function)

function의 경우 지역이건 전역이건 어떠한 기능을 수행하는 함수를 의미한다. 반면, method의 경우 객체 또는 클래스 내부에서 선언된 함수를 의미한다.

> 모든 메서드는 함수이지만, 모든 함수는 메서드가 아니다.
> 

# this

Javascript는 함수 또는 블록 레벨에서 범위를 결정하는 lexical scope 규칙을 따르지만, this는 dynamic scope 방식을 따르기 때문에 주의해야 한다.

## Arrow function에서의 this

![image2019-9-29_15-11-21](https://user-images.githubusercontent.com/58387974/135084955-83420365-adf4-4b5e-9bf8-315ea49e827d.png)

일반적인 경우 this는 함수를 선언할 때, this에 바인딩할 객체가 정적으로 결정되는 것이 아니라, 호출과 동시에 this에 바인딩할 객체가 동적으로 결정된다.

### 일반적인 Function this

```jsx
const obj = {
	name: "Eddy",
	goodBoy: function() {
		console.log(`goodBoy result: ${this.name}`);
	}
}

obj.goodBoy(); // goodBoy result: Eddy
```

만약 this가 메서드 더 안에 있다면 어떻게 될까?

### inner method

```jsx
const obj = {
	name: "Eddy",
	strengths: ["kind1", "kind2...", "kind3..."],
	goodBoy: function() {
		console.log(`goodBoy result: ${this.name}`);
		this.strengths.forEach(function(strength) {
			console.log(`${this.name}'s strengths is ${strength}`);
		})
	}
}

obj.goodBoy(); // goodBoy result: Eddy /n TypeEror: Cannot read property 'name' of undefined
```

일반 함수의 사용시에 범위는 기본적으로 전역 함수에 바인딩된다.

strict 모드에서는 error가 나오지만, 일반 모드에서는 undefined가 나오며, 그냥 console.log(this);를  출력하면 window 객체가 나온다.

이렇게 나오는 이유는

1. this는 기본적으로 현재 함수의 소유자를 참조하기 때문이다. 익명 함수의 경우 this는 전역범위에 있는 window 객체를 바인딩한다. 그렇기 때문에 error나 window 객체가 나오는 현상이 일어난다.
2. 중첩된 구조에서는 각각 dynamic scope가 모두 발생한다.

# Scope Model

## Dynamic Scope

변수나 함수가 불려진 곳의 context를 사용한다.

콜 스택에서 먼저 검색한다. 콜 스택의 데이터 구조는 LIFO

다이나믹 스코프는 어디에서 선언되는지가 아니라, 어디에서 호출되는지에 관심이 있다. 그래서 스택이 보유하는 변수에 따라 런타임시에 동적으로 결정될 수 있는 것이다.

## Lexical Scope

변수나 함수가 정의된 곳의 context를 사용한다. Javascript가 사용하는 스코프이기도 하다.

(but, 자바스크립트에서 일반 함수는 다이나믹 스코프와 비슷한 방식으로 작동하며, context는 함수 호출 방법에 따라 런타임에 정의 된다.)

다른 함수 내에 함수를 정의하면, 내부 함수는 내부 함수의 런타임에 관계없이 항상 외부 함수의 범위에 정의된 모든 변수에 접근할 수 있다. 자바스크립트에서 화살표 함수를 사용하면 런타임시에 동적으로 결정되는 것이 아니라, Lexical Scope에 따라 결정된다.

### Lexical Scope인 Arrow Function

```jsx
const Info = {};
Info.name = 'Eddy';

Info.introduce = () => {
	console.log(`I'm ${this.name}.`)
}

Info.introduce(); // I'm undefined
```

- 빈 객체를 리터럴을 생성하고 나중에 속성을 지정하는 것과 같기 때문에 결과 값으로 undefined가 출력된다.

### Arrow Function this

```jsx
const obj = {
	name: "Eddy",
	strengths: ["kind1", "kind2...", "kind3..."],
	goodBoy: function() {
		console.log(`goodBoy result: ${this.name}`);
		this.strengths.forEach((strength) => {
			console.log(`${this.name}'s strengths is ${strength}`);
		})
	}
}

obj.goodBoy();

// results
// goodBoy result: Eddy
// Eddy's strengths is kind1
// Eddy's strengths is kind2...
// Eddy's strengths is kind3...
```

화살표 함수는 자체 범위를 바인딩하지 않지만 부모 또는 범위를 참조한다.