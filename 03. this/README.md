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
