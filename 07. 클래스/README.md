# 07. 클래스
## 7-1 클래스와 인스턴스의 개념 이해
**상위 클래스** : 어떤 사물들의 공통 속성을 모아 정의한 것, 추상적인 개념  
**인스턴스** : 어떤 클래스의 속성을 지니는 실존하는 개체  
ex) 과일 → 배, 사과, 바나나  

<img width="333" alt="스크린샷 2023-03-21 오후 12 12 45" src="https://user-images.githubusercontent.com/101851472/226531861-1bd01174-cab5-4b6c-a753-7e1213704342.png">

**superClass(상위 클래스) ⇒ subClas(하위 클래스)**  
클래스는 하위로 갈수록 상위 클래스의 속성을 상속하면서 더 구체적인 요건이 추가 또는 변경됨  

## 7-2 자바스크립트의 클래스  
자바스크립트는 **프로토타입 기반 언어**  
► 인스턴스에 상속되는지(인스턴스가 참조하는지) 여부  
1.  **스태틱 멤버(static member)** : 인스턴스에서 직접 접근할 수 없는 메서드  
    - 생성자 함수를 this로 해야만 호출 가능!  
2. **인스턴스 멤버(instancee member)** : 자바스크립트에서는 인스턴스에서도 직접 메서드 정의 가능!  
  ⇒ **프로토타입 메서드** : 인스턴스에서 직접 호출할 수 있는 메서드  
  
> 스태틱 메서드, 프로토타입 메서드
```jsx
var Rectangle = function (width, height){ // 생성자
	this.width = width;
	this.height = height;
};
Rectabgle.prototype.getArea = function (){ // (프로토타입) 메서드
	return this.width * this.heigth;
};
Rectabgle.isRectangle = function (instance){ // 스태틱 메서드
	return instance instanceof Rectangle && 
		instance.width > 0 && instance.height > 0;
};

var rect1 = new Rectangle(3, 4)
console.log(rect1.getArea()); // 12 (o) (프로토타입 메서드)
console.log(rect1.isRectangle(rect1)); // Error (x)
console.log(Rectangle.isRectangle(rect1)); // true
```
<img width="507" alt="스크린샷 2023-03-21 오후 3 16 12" src="https://user-images.githubusercontent.com/101851472/226532140-57c725f9-e3d7-4426-8fcd-ddd82aeebdec.png">

## 7-4 ES6의 클래스 및 클래스 상속
ES6 → 클래스 문법 도입!  
> ES5와 ES6의 클래스 문법 비교  
```jsx
var ES5 = function (name){
	this.name = name;
};
ES5.staticMethod = function (){
	return this.name + ' method';
};
ES5.prototype.method = function (){
	return this.name + ' method';
};
var es5Instance = new ES5('es5');
console.log(ES5.staticMethod()); // es5 staticMethod
console.log(es5Instance.method()); // es5 method
---------------------------------
var ES6 = class { // 클래스 본문 영역
	constructor (name) { // function 키워드 생략해도 메서드로 인식 / 생성자 함수와 동일한 역할
		this.name = name;
	}
	static staticMethod () { // static 메서드임을 알림 / 생성자 함수 자신만이 호출 가능
		return this.name + ' staticMethod';
	}
	method () { // 자동으로 prototype 객체 내부에 할당되는 메서드
		return this.name + ' method';
	}
};
var es6Instance = new ES6('es6');
console.log(ES6.staticMethod()); // es6 staticMethod
console.log(es6Instance.method()); // es6 method
```

> ES6의 클래스 상속  
```jsx
var Rectangle = class {
	constructor (width, heigth) {
		this.width = width;
		this.heigth = heigth;
	}
	getArea (){
		return this.width * this.heigth;
	}
};
var Square = class extends Rectangle { // 상속 관계 설정
	constructor (width) {
		super(width, width); // superClass의 constructor를 실행함
	}
	getArea() {
		console.log('size is : ', super.getArea());
	}
}
```
**extends** 키워드 ⇒ **상속 관계** 설정 (Square는 Rectangle을 상속받는 SubClass)  
