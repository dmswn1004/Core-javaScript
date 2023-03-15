# 03. this
## 3-1 상황에 따라 달라지는 this  
this는 기본적 **실행 컨텍스트가 생성**될 때 결정된다.  
⇒ this는 기본적으로 **함수를 호출할 때 결정**된다.

#### 3-1-1 전역 공간에서의 this
전역 공간에서의 this는 **전역객체**를 가리킨다.  
- **전역객체** : 전역 컨텍스트 생성 주체(브라우저 : **window** , node.js : **global**)  
(참고) 자바스크립트의 모든 변수는 실은 **특정 객체의 프로퍼티(실행 컨텍스트의 LexicalEnvironment)** 로서 동작한다.

> 전역변수와 전역객체 선언

```jsx
var a = 1;
window.b = 2;
console.log(a, window.a, this.a); // 1 1 1
console.log(b, window.b, this.b); // 2 2 2
```

> 전역변수와 전역객체 삭제

```jsx
var a = 1;
delete window.a; // (delete a;) false

console.log(a, window.a, this.a); // 1 1 1

window.b = 3;
delete window.c; // (delete c;) ture
console.log(c, window.c, this.c); // Uncauht ReferenceError: c in not defined
```

- 전역 객체의 프로퍼티로 할당한 경우 → 삭제 O
- 전역변수로 선언한 경우 → 삭제 X
    - configurable(변경 및 삭제 가능성) 속성 false로 정의됨

#### 3-1-2 메서드로서 호출할 때 그 메서드 내부에서의 this
**함수 vs 메서드**  

▶︎ **함수를 실행하는 방법**  
1. **함수**로서 호출하는 경우  
2. **메서드**로서 호출하는 경우   

💡 **함수와 메서드의 차이**  
(**함수, 메서드** : 미리 정의한 동작을 수행하는 코드 뭉치) → 둘의 차이는 **독립성**  
- **함수** : 그 자체로 독립적인 기능을 수행
- **메서드** : 자신을 호출한 대상 객체에 관한 동작을 수행

> 함수로서 호출, 메서드로서 호출

```jsx
var func = function (x) {
	console.log(this, x);
};
func(1); // window { ... } 1

var obj = {
	method: func
};
obj.method(2); // { method: f } 2
obj['method'](2); // { method: f } 2
```

**메서드 내부에서의 this**  
- this에는 **호출한 주체**에 대한 정보가 담김 ⇒ **호출 주체** : 함수명(프로퍼티명) 앞의 객체

#### 3-1-3 함수로서 호출할 때 그 함수 내부에서의 this
**함수 내부에서의 this**. 
함수를 함수로서 호출한 경우에는 호출 주체의 정보를 알 수 없기 때문에 this가 지정되지 않았기 때문에 this는 **전역 객체**를 가리킨다.

**메서드의 내부 함수에서의 this**  
> 내부 함수에서의 this  
```jsx
var obj = {
	outer: function() {
		console.log(this); // obj1
		var innerFunc = function (){
			console.log(this); // window
		}
		innerFunc(); // 함수로서 호출

		var obj2 = {
			innerMethod: innerFunc // obj2
		};
		obj2.innerMethod(); // 메서드로서 호출
	}
};
obj1.outer(); // 메서드로서 호출
```

**메서드의 내부 함수에서의 this를 우회하는 방법**  
> 내부 함수에서의 this를 우회하는 방법
```jsx
var obj = {
	outer: function() {
		console.log(this);  // {outer: f}
		var innerFunc1 = function (){
			console.log(this); // window
		}
	};
	innerFunc1();

	var self = this;
	var innerFunc2 = function() {
		console.log(self); // {outer: f}
	};
};
obj.outer();
```

**this를 바인딩 하지 않는 함수**  
this를 바인딩 하지 않는 **화살표 함수(arrow function)** 도입 (화살표 함수는 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지게 되어, 상위 스코프의 this를 그대로 활용 가능)

#### 3-1-4 콜백 함수 호출 시 그 함수 내부에서의 this
**콜백 함수** : 함수 A의 제어권을 다른 함수(또는 메서드) B에게 넘겨주는 경우 제어권을 넘겨준 함수 A  
- 함수 A는 함수 B의 내부 로직에 따라 실행  

#### 3-1-5 생성자 함수 내부에서의 this
**생성자 함수** : 어떤 공통된 성질을 지니는 객체들을 생성하는 데 사용하는 함수  
객체지향 언어 ⇒ 클래스(생성자), 인스턴스(클래스를 통해 만든 객체)
⇒ **생성자** : 구체적인 인스턴스를 만들기 위한 틀  
사용법 : new 명령어와 함께 함수를 호출 → 생성자의 prototype 프로퍼티를 참조하는 __proto__라는 프로퍼티가 있는 객체 생성 → 미리 준비된 공통 속성 및 개성을 해당 객체(this)에 부여

```jsx
var Cat = function (name, age) {
	this.name = name;
	this.age = age;
};

var choco = new Cat("초코", 7);
var nabi = new Cat("나비", 5);
console.log(choco, nabi); 

// Cat{ name:'초코', age:7 }
// Cat{ name:'나비', age:5 }
```

## 3-2 명시적으로 this를  바인딩 하는 방법
#### 3-2-1 call 메서드
```jsx
Function.prototype.call(thisArg[, arg1[, arg2[,...]]])
```
**call 메서드** : 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령  
- call 메서드의 **첫 번째 인자를 this로 바인딩**, 이후의 인자들을 호출할 함수의 매개변수로 함

> call 메서드
```jsx
var func = function (a, b, c) {
	console.log(this, a ,b ,b);
};
func(1, 2, 3); // window{ ... } 1 2 3
func.call({x: 1}, 4, 5, 6); // { x: 1 } 4 5 6

var obj = {
	a: 1,
	method: function (x, y) {
		console.log(this.a, x, y);
	}
};
obj.method(2, 3); // 1 2 3
obj.method.call({ a: 4 }, 5, 6); // 4 5 6
```

#### 3-2-2 apply 메서드
```jsx
Function.prototype.apply(thisArg[, argsArray])
```
**apply 메서드** : call 메서드와 기능적으로 동일  
- **두 번째 인자를 배열로 받아** 그 배열의 요소들을 호출할 함수의 매개변수로 지정

> apply 메서드
```jsx
var func = function (a, b, c) {
	console.log(this, a ,b ,b);
};
func.apply({x: 1}, [4, 5, 6]); // { x: 1 } 4 5 6

var obj = {
	a: 1,
	method: function (x, y) {
		console.log(this.a, x, y);
	}
};
obj.method.apply({ a: 4 }, [5, 6]); // 4 5 6
```

#### 3-2-3 call / apply 메서드의 활용

🔍 **유사 배열 객체에 배열 메서드를 적용**  
**유사 배열 객체** : **키가 0 또는 양의 정수인 프로퍼티**가 존재하고 **length 프로퍼티**의 값이 0 또는 양의 정수인 객체 (배열의 구조와 유사한 객체의 경우)  
> Array.from 메서드  
: **유사 배열 객체 또는 순회 가능한 모든 종류의 데이터 타입**을 배열로 전환하는 메서드  
```jsx
var obj = {
	0: 'a',
	1: 'b',
	2: 'c',
	length: 3
};
var arr = Array.from(obj);
console.log(arr); // ['a', 'b', 'c']
```

🔍 **생성자 내부에서 다른 생성자를 호출**  
> call/apply 메서드의 활용
```jsx
function Person(name, gender) {
	this.name = name;
	this.gender = gender;
}

function Student(name, gender, school) {
	Person.call(this, name, gender);
	this.school = school;
}
function Employee(name, gender, company) {
	Person.apply(this, [name, gender]);
	this.company = company;
}
var by = new Student('보영', 'fmale', '단국대');
var jn = new Employee('재난', 'male', '구골');
```
Student, Employee 생성자 함수 내부에서 Person 생성자 함수를 호출해서 인스턴스 속성을 정의 → 반복 줄어든다.

🔍 **여러 인수를 묶어 하나의 배열로 전달하고 싶을 때 - apply 활용**  
```jsx
var numbers = [10,20,3,16,45];
// var max = Math.max.apply(null, numbers);
// var min = Math.min.apply(null, numbers);
const max = Math.max(...numbers);
const min = Math.min(...numbers);
console.log(max, min);  // 45 3
```
**펼치기 연산자(spread operator)** : …

#### 3-2-4 bind 메서드
```jsx
Fnuction.prototype.bind(thisArg[, arg1, [arg2[, ...]])
```
**bind 메서드** : call과 비슷하지만 즉시 호출하지는 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 하는 메서드  
→ 함수에 this를 미리 적용하는 것과 부분 적용 함수를 구현하는 두 가지 목적  
> bind 메서드 - this 지정과 부분 적용 함수 구현  
```jsx
var func = function (a, b, c, d){
	console.log(this, a, b, c,d);
};
func(1, 2, 3, 4);  // window{ ... } 1 2 3 4

var bindFunc1 = func.bind({x: 1});
bindFunc1(5, 6, 7, 7);  // { x: 1 } 5 6 7 8

var bindFunc2 = func.bind({ x: 1 }, 4, 5);
bindFunc2(6, 7);  // { x: 1 } 4 5 6 7
bindFunc2(8, 9);  // { x: 1 } 4 5 8 9
```

🔍 **name 프로퍼티**  
name 프로퍼티에 동사 bind의 수동태인 **‘bound’** 라는 접두어가 붙는다!  
```jsx
var func = function (a, b, c, d){
	console.log(this, a, b, c,d);
};
var bindFunc = func.bind({ x: 1 }, 4, 5);
console.log(func.name);  // func
console.log(bindFunc.name);  // bound func
```

🔍 **상위 컨텍스트의 this를 내부 함수나 콜백 함수에 전달하기**  
메서드의 내부 함수에서 메서드의 this를 그대로 바라보게 하기 위한 방법으로 self를 사용하는 것보다 call, apply, bind 메서드를 이용해 더 깔끔하게 처리 가능  

#### 3-2-5 화살표 함수의 예외사항  
**화살표 함수**는 this를 바인딩 하는 과정이 제외 → 함수 내부에는 this가 아예 없음, 접근하고자 하면 스코프체인상 가장 가까운 this에 접근  

#### 3-2-6 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)  
콜백 함수를 인자로 받는 메서드 중 일부는 추가로 this로 지정할 객체(**thisArg**)를 인자로 지정할 수 있는 경우가 있는데, 이러한 메서드의 thisArg 값을 지정하면 콜백 함수 내부에서 this 값을 원하는 대로 변경할 수 있다. 이런 형태는 여러 내부 요소에 대해 같은 동작을 반복 수행해야 하는 **배열 메서드**에 많이 포진  

> 콜백 함수와 함께 thisArg를 인자로 받는 메서드  
```jsx
Array.prototype.forEach(callback[, thisArg])
Array.prototype.map(callback[, thisArg])
Array.prototype.filter(callback[, thisArg])
Array.prototype.some(callback[, thisArg])
Array.prototype.every(callback[, thisArg])
Array.prototype.find(callback[, thisArg])
Array.prototype.findIndex(callback[, thisArg])
Array.prototype.flatMap(callback[, thisArg])
Array.prototype.from(arrayLike[, callback[, thisArg]])
Set.prototype.forEach(callback[, thisArg])
Map.prototype.forEach(callback[, thisArg])
```
