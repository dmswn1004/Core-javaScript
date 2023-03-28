# 04. 콜백 함수
## 4-1 콜백 함수란?
**콜백 함수 (callback function)** : 다른 코드의 인자로 넘겨주는 함수  
→ **콜백 함수**를 넘겨받은 코드는 **필요에 따라 적절한 시점에 실행**할 것!  
⇒ **콜백 함수**는 다른 코드(함수 또는 메서드)에게 인자로 넘겨줌으로써 그 **제어권**도 함께 위임하는 함수  

## 4-2 제어권
#### 4-2-1 호출 시점
> setInterval 예제
```jsx
var count = 0;
var timer = setInterval(function () { 
		console.log(count);
		if(++count > 4) clearInterval(timer);
}, 300) // timer = setInterval의 ID 값
```
> setInterval 구조
```jsx
var intervalID = scope.setInterval(func,delay[, param1, param2, ...]);
```
매개변수 : **func(함수), delay(ms 단위의 숫자)**, 나머지(param1, param2, …)  
⇒ func에 넘겨준 함수는 매 delay마다 실행되며, 그 결과 어떠한 값도 리턴 X, setInterval를 실행하면 반복적으로 실행되는 내용 자체를 특정할 수 있는 고유한 ID 
값이 반환, 변수에 담는 이유는 반복 실행되는 중간에 종료(**clearInterval**) 할 수 있게 하기 위해서이다.

#### 4-2-2 인자
```jsx
Array.prototype.map(callback[, thisArg])
callback: function(currentValue, index, array)
```
**map 메서드**  
: 대상이 되는 배열의 모든 요소들을 처음부터 끝까지 하나식 꺼내어 콜백 함수를 반복 호출하고, 콜백 함수의 실행 결과들을 모아 **새로운 배열 반환**   
: **첫 번째 인자로 callback 함수**를 받고, 생략 가능한 **두 번째 인자로 콜백 함수 내부에서 this로 인식할 대상**을 특정할 수 있다. thisArg를 생략할 경우, 일반적인 함수와 마찬가지로 **전역 객체**가 바인딩 된다.  

- **function(currentValue, index, array)**
    - **호출하는 주체**는 **map 메서드** ⇒ **제어권**도 **map 메서드**에게 있음!!

#### 4-2-2 this
> 콜백 함수 내부에서의 this
```jsx
setTimeout(function () { console.log(this); }, 300); // window { ... }
```

## 4-3 콜백 함수는 함수다
⇒ 콜백 함수로 **어떤 객체의 메서드를 전달하더라도** 메서드가 아닌 **함수로서 호출됨**!  
> 메서드를 콜백 함수로 전달한 경우  
```jsx
var obj = {
	vals: [1,2,3],
	logValuses: function(v, i) {
		console.log(this, v, i);
	}
};
obj.logValues(1, 2); // 메서드로서 호출 => this : obj
// {vals: [1, 2, 3], logValues: f} 1 2
[4, 5, 6].forEach(obj.logValues); // Window { ... } 4 0 -> 5 1 -> 6 2 
```

## 4-4 콜백 함수 내부의 this에 다른 값 바인딩하기
> 콜백 함수 내부에 this에 다른 값을 바인딩하는 방법 → 전통적인 방식  
```jsx
var obj1={
	name:'obj1',
	func: function(){
		var self = this; 
		return function(){
			console.log(self.name); //this == self == obj1
		};
	}
};
var callback = obj1.func();
setTimeout(callback,1000); // 출력 : obj1
```

> 콜백 함수 내부에서 this를 사용하지 않은 경우
```jsx
var obj1={
	name: 'obj1',
	func: function(){	
		console.log(obj1.name); 
	}
};
setTimeout(obj1.func,1000);
```
⇒ this를 이용해 다양한 상황에 **재활용 불가** (바라볼 객체를 명시적으로 obj1로 지정해서)  
> func 함수 재활용 가능  
```jsx
...
var obj2={
	name: 'obj2',
	func: obj1.func
};
var callback2 = obj2.func();
setTimeout(callback2,1500); // obj2

var obj3 = {name: 'obj3'};
var callback3 = obj1.func.call(obj3);
setTimeout(callback3, 2000); // obj3
```

⇒ 방법은 번거롭지만, **this를 우회적으로 활용**해 다양한 상황에서 원하는 객체를 바라보는 콜백 함수 만들기 가능  

## 4-5 콜백 지옥과 비동기 제어
**콜백 지옥** : 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상  

**비동기 ↔ 동기**  
- **동기**적인 코드 : **현재 실행 중인 코드가 완료된 후**에야 다음 코드를 실행하는 방식  
    - **예)** CPU의 계산에 의해 즉시 처리가 가능한 대부분의 코드  
- **비동기적**인 코드 : **현재 실행 중인 코드의 완료 여부와 무관하게** 즉시 다음 코드로 넘어감  
    - setTimeout, addEventListener, XMLHttpRequest 등  
    - 별도의 요청, 실행 대기, 보류 등과 관련된 코드  
    
> 콜백 지옥
```jsx
setTimeout(function (name)){  //  ---- (4)
	var coffeeList = name;
		console.log(coffeeList);

		setTimeout(function (name)){  // ---- (3)
		  coffeeList += ',' + name;
			console.log(coffeeList);

			setTimeout(function (name)){  // ---- (2)
			  coffeeList += ',' + name;
				console.log(coffeeList);
	
				setTimeout(function (name)){  // ----(1)
				  coffeeList += ',' + name;
					console.log(coffeeList);
				},500,'카페라떼');
			},500,'카페모카');
		},500,'아메리카노');
},500,'에스프레소');
```
→ 0.5초 주기마다 커피 목록을 수집하고 출력하는 코드  
⇒ 가독성 문제 & 어색함 해결 : 익명의 콜백 함수를 모두 **기명함수**로 전환  

> 콜백 지옥 해결 - 기명함수로 변환  
```jsx
var coffeeList='';

var addEsspresso = function (name) {
	coffeeList = name;
	console.log(coffeeList);
	setTimeout(addAmericano,500,'아메리카노');
};

var addAmericano = function (name) {
	coffeeList += ',' + name;
	console.log(coffeeList);
	setTimeout(addMocha,500,'카페모카');
};

var addMocha = function (name) {
	coffeeList += ',' + name;
	console.log(coffeeList);
	setTimeout(addLatte,500,'카페라떼');
};

var addLatte = function (name) {
	coffeeList += ',' + name;
	console.log(coffeeList);;
};

setTimeout(addEspresso,500,'에스프레소');
```
→ 코드의 가독성 ↑  


자바스크립트에서 **비동기적인 작업을 동기적으로, 혹은 동기적인 것처럼 보이게끔 처리**해 주는 장치 마련 ⇒ **Promise, Generator , async/await** 도입  

> 비동기 작업의 동기적 표현 - Promise
> 

```jsx
new Promise(function (resolve){
	setTimeout(function(){
		var name='에스프레소';
		console.log(name);
		resolve(name);
	},500);
}).then(function(prevName){
	return new Promise(function (resolve){
		setTimeout(function(){
			var name=prevName + ', 아메리카노';
			console.log(name);
			resolve(name);
		},500);
	});
}).then(function(prevName){
	return new Promise(function (resolve){
		setTimeout(function(){
			var name=prevName + ', 카페모카';
			console.log(name);
			resolve(name);
		},500);
	});
}).then(function(prevName){
	return new Promise(function (resolve){
		setTimeout(function(){
			var name=prevName + ', 카페라떼';
			console.log(name);
			resolve(name);
		},500);
	});
});
```

⇒ new 연산자와 함께 호출한 Promise의 인자로 넘겨주는 콜백 함수는 호출할 때 실행되지만 내부에 **resolve 또는 reject 함수**를 호출하는 구문이 있을 경우 둘 중 하나가  실행되기 전까지는 다음(then) 또는 오류 구문(catch)으로 넘어가지 않음

> 비동기 작업의 동기적 표현 - Promise ⇒ 반복직인 내용 함수화해 짧게 표현한 코드
> 

```jsx
var addCoffee = function(name){
	return function(prevName){
		return new Promise(function (resolve){
			setTimeout(function(){
				var newName = prevName ? (prevName + ' ,'+name) : name;
				console.log(newName);
				resolve(newName);
			},500);
		});
	};
};
addCoffee('에스프레소')()
	.then(addCoffee('아메리카노'))
	.then(addCoffee('카페모카'))
	.then(addCoffee('카페라떼'))
```

> 비동기 작업의 동기적 표현 - Generator  
```jsx
var addCoffee = function(prevName, name) {
	setTimeout(function() {
		coffeeMaker.next(prevName ? prevName + ', ' + name : name);
	}, 500);
};
var coffeeGenerator = function* () { // '*'이 붙은 함수 -> Generator 함수
	var espresso = yield addCoffee('', '에스프레소');
	console.log(espresso);
	var americano = yield addCoffee(espresso, '아메리카노');
	console.log(americano);
	var mocha = yield addCoffee(americano, '카페모카');
	console.log(mocha);
	var latte = yield addCoffee(mocha, '카페라떼');
	console.log(latte);
};
var coffeeMaker = coffeeGenerator();
coffeeMaker.next();
```
Generator 함수를 실행하면 **Iterator가 반환** ⇒ Iterator가 가지고 있는 **next 메서드**를 호출하면 Generator 함수 내부에서 가장 먼저 등장하는 **yield**에서 함수의 실행을 중지함  

> 비동기 작업의 동기적 표현 - Promise + Async/await
```jsx
var coffee = function(name){
	return new Promise(function (resolve){
		setTimeout(function(){
			resolve(name);
		},500);
	});
};
var coffeeMaker = async function(){
	var coffeeList = '';
	var _addCoffee = async function (name) {
		coffeeList += (coffeeList ? ',':'') + await addCoffee(name);
	};
	await _addCoffee('에스프레소');
	console.log(coffeeList);
	await _addCoffee('아메리카노');
	console.log(coffeeList);
	await _addCoffee('카페모카');
	console.log(coffeeList);
	await _addCoffee('카페라떼');
	console.log(coffeeList);
};
coffeeMaker();
```
⇒ 비동기 작업을 수행하고자 하는 함수 앞에 **async**를 표기, 함수 내부에서 실질적인 비동기 작업이 필요한 위치마다 **await**를 표기만으로 뒤의 내용을 Promise로 자동 전환, 해당 내용이 resolve된 이후에야 다음으로 진행  
