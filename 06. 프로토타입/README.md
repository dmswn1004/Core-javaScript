# 06. 프로토타입

자바스크립트 ⇒ **프로토타입(prototype)** 기반 언어  
→ 어떤 객체를 원형(prototype)으로 삼고 이를 복제(참조)  

## 6-1 프로토타입의 개념 이해

#### 6-1-1 constructor, prototype, instance
<div>
  <img width="281" alt="스크린샷 2023-03-13 오전 3 26 08" src="https://user-images.githubusercontent.com/101851472/224783537-0eeb4879-d256-4d74-9120-1b8fbcc7ad33.png">
  <img width="281" alt="스크린샷 2023-03-13 오전 3 26 00" src="https://user-images.githubusercontent.com/101851472/224783547-f18f0601-4001-4765-b8da-06b6819179c7.png">
</div>

```jsx
var instance = new Constructor();
```

**흐름 파악**
- **어떤 생성자 함수(Constructor)** 를 **new 연산자** 와 함께 호출하면 Coustructor에서 정의된 내용을 바탕으로 새로운 **인스턴스(instance)** 가 생성된다.  
- 이때 instance에는 __proto__라는 프로퍼티가 자동으로 부여되는데, 이 프로퍼티는 Constructor의 prototype이라는 프로퍼티를 참조한다.  

**prototype** : 객체 → **__proto__** : 객체 (참조했으니까…)  
prototype 객체 내부에는 인스턴스가 사용할 메서드를 저장

> Person.prototype
```jsx
var Person = function (name) {
	this._name = name;
};
Person.prototype.getName = function() {
	return this._name;
};
var suzi = new Person('Suzi');
suzi.__proto__.getName(); // undefined (이유: __proto__ 객체에 name 프로퍼티가 없어서) 
// ⇒ **메서드**로서 호출 : 바로 앞의 객체가 this → suzi.__proto__ 

suzi.__proto__.name = 'suzi.__proto__';
suzi.__proto__.getName(); // suzi.__proto__ (__proto__ 객체에 name 프로퍼티에 할당 후)
```

(참고) __proto__는 생략 가능한 프로퍼티  
**정리** : new 연산자로 Constructor를 호출하면 instance가 만들어지는데, 이 instance의 생략가능한 프로퍼티인 __proto__는 Constructor의 prototype을 참조한다.  

⇒ 상세 설명 : 함수에 자동으로 객체인 prototype 프로퍼티를 생성해 놓는데, 해당 함수를 생성자 함수로써 사용할 경우, 즉 new 연산자와 함께 함수를 호출할  
경우, 그로부터 생성된 인스턴스에는 숨겨진 프로퍼티인 __proto__가 자동으로 생성되며, 이 프로퍼티는 생략 가능하도록 구현돼 있기 때문에 생성자 함수의 
prototype에 어떤 메서드나 프로퍼티가 있다면 인스턴스에도 마치 자신의 것처럼 해당 메서드나 프로퍼티에 접근할 수 있게 됩니다.  

#### 6-1-2 constructor 프로퍼티
- 옅은색 : **innumerable** (열거할 수 없는 프로퍼티)  
- 짙은색 : **enumerable** (열거 가능한 프로퍼티)  
<img width="421" alt="스크린샷 2023-03-13 오후 5 53 34" src="https://user-images.githubusercontent.com/101851472/224785676-1bd8796e-b8d2-4ccf-89dc-c241b15b08e5.png">

생성자 함수의 프로퍼티인 prototype 객체 내부에 constructor 프로퍼티 존재 (__proto__에도 존재)  
→ 생성자 함수(자기 자신) 참조
⇒ 이유 : 인스턴스로부터 그 원형이 무엇인지 알 수 있는 수단  
<img width="283" alt="스크린샷 2023-03-14 오전 3 11 32" src="https://user-images.githubusercontent.com/101851472/224793975-4e8dc299-9537-410a-a04d-98e88c4851f9.png">

> 모두 동인한 대상을 가리키는 코드
```jsx
[Constructor]
[instance].__proto__.constructor
[instance].constructor
Object.getPrototypeOf([instance]).constructor
[Constructor].prototype.constructor
```

> 모두 동일한 객체(prototype) 접근 가능
```jsx
[Constructor].prototype
[instance].__proto__
[instance]
Object.getPrototypeOf([instance])
```
