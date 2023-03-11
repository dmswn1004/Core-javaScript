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
this를 바인딩 하지 않는 **화살표 함수(arrow function)** 도입 (화살표 함수는 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지게 되어, 상위 스코프의 this를 그래도 활용 가능)

### 3-1-4 콜백 함수 호출 시 그 함수 내부에서의 this
**콜백 함수** : 함수 A의 제어권을 다른 함수(또는 메서드) B에게 넘겨주는 경우 제어권을 넘겨준 함수 A  
- 함수 A는 함수 B의 내부 로직에 따라 실행  

### 3-1-5 생성자 함수 내부에서의 this
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
