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

