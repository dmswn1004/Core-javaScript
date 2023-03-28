# 05. 클로저
## 5-1 클로저의 의미 및 원리 이해
**클로저(Closure)** : 어떤 함수에서 선언한 변수를 참조하는 내부 함수를 외부로 전달할 경우, 함수의 실행 컨텍스트가 종료된 후에도 해당 변수가 사라지지 않는 현상    
> 외부 함수의 변수를 참조하는 내부 함수(1) - 그림 5-1
```jsx
var outer = function () {   // 외부 함수
  var a = 1;
  var inner = function () { // 내부 함수
    console.log(++a); // 2
  };
  inner();
};
outer();
```

> 외부 함수의 변수를 참조하는 내부 함수(3) - 그림 5-2
```jsx
var outer = function () {   // 외부 함수
  var a = 1;
  var inner = function () { // 내부 함수
    return ++a;
  };
  return inner();
};
var outer2 = outer();
console.log(outer2()); // 2
console.log(outer2()); // 3
```

<div>
  <img width="403" alt="스크린샷 2023-03-20 오전 2 17 23" src="https://user-images.githubusercontent.com/101851472/226197319-3ae682b9-4fdb-4518-8b8d-936b0527f618.png">
  <img width="403" alt="스크린샷 2023-03-20 오전 2 31 35" src="https://user-images.githubusercontent.com/101851472/226197364-123835f5-7c04-4b4f-bf2a-e5ea2ac826a2.png">
 
</div>

a를 참조하는 변수가 하나도 없게 되면 **가비지 컬렉터(GC)의 수집 대상**이 되어 삭제된다. 만약 호출될 가능성이 있는 경우에는 수집 대상에서 제외된다.

## 5-2클로저와 메모리 관리
- **메모리 누수** : 개발자의 의도와 달리 **어떤 값의 참조 카운트가 0이 되지 않아** 가비지 컬렉터의 수거 대상이 되지 않는 경우  
- **메모리 소모** ⇒ 클로저의 본질적인 특성 (의도 대로 설계한 **메모리 소모**에 대한 관리법만 잘 파악해서 적용하는 것으로 충분)  

  - 관리법 : 필요성이 사라진 시점에 메모리를 차지하지 않도록 참조 카운트를 0으로 만든다!  
  ⇒ 참조 카운트 0으로 만드는 방법 : 식별자에 참조형이 아닌 **기본형 데이터(null, undefined) 할당**

## 5-3 클로저 활용 사례
#### 5-3-1 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때
1. 콜백 함수를 내부함수로 선언해서 외부변수를 직접 참조하는 방법 (클로저 사용)  
    ```jsx
    var fruits = ['apple', 'banana', 'peach'];
    var $ul = document.createElement('ul');
    fruits.forEach(function (fruit){
    	var $li = document.createElement('li');
    	$li.innerText = fruit;
    	$li.addEventListener('click', function) (){
    		alert('your choice is ' + fruit); // 클로저 o
    	});
    	$ul.appendChild($li);
    });
    document.body.appendChild($ul);
    ``` 
2. bind 메서드로 값을 직접 넘겨줌 (클로저 사용 x, 제약사항)  
    ```jsx
    ...
    fruits.forEach(function (fruit){
    	var $li = document.createElement('li');
    	$li.innerText = fruit;
    	$li.addEventListener('click', alertFruit.bind(null, fruit));
    	$ul.appendChild($li);
    });
    ...
    ```  
3. 콜백 함수를 고차함수로 바꿈 (클로저 적극적으로 활용)
    ```jsx
    var alertFruitBuilder = function (fruit) {
    	return function () {
    		alert('your choice is ' + fruit);
    	};
    };
    fruits.forEach(function (fruit){
    	var $li = document.createElement('li');
    	$li.innerText = fruit;
    	$li.addEventListener('click', alertFruitbuilder(fruit));
    	$ul.appendChild($li);
    });
    ...
    ```
    
#### 5-3-2 접근 권한 제어(정보 은닉)
**정보 은닉(Information hiding)** : 어떤 모듈의 내부 로직에 대해 외부로의 노출을 최소화해서 모듈 간의 결합도를 낮추고 유연성을 높이고자 하는 개념  
- **public** : 외부에서 접근 가능  
- **private** : 내부에서만 사용하며 외부에 노출되지 않는 것  
- **protected**  

자바스크립트는 기본적으로 변수 자체에 이러한 접근 권한 직접 부여하지 못함  
⇒ **return 이용해** 함수 내부의 변수들 중 선택적으로 일부의 변수에 대한 접근 권한을 부여할 수 있음  
외부에 제공하고하는 정보들을 모아서 return하고, 내부에서만 사용할 정보들은 return 하지 않은 것으로 접근 제어 가능  


💡 **클로저**를 활용해 **접근권한을 제어**하는 방법

1. 함수에서 지역변수 및 내부함수 등을 생성  
2. 외부에 접근권한을 주고자 하는 대상들로 구성된 **참조형 데이터**(대상이 여럿일 때는 객체 또는 배열, 하나일 때는 함수)를 **return**한다.    
    ⇒ **return한 변수들**은 **공개 멤버**가 되고, 그렇지 않은 변수들은 **비공개 멤버**가 된다.   

#### 5-3-3 부분 적용 함수
**부분 적용 함수(partially applied function)** : n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가, 나중에 (n-m)개의 인자를 넘기면 비로소 원래 함수의 실행 결과를 얻을 수 있게끔 하는 함수  
⇒ 미리 인자를 넘겨두어 기억하게끔 하고 추후 피리요한 시점에 기억했던 인자들까지 함께 실행하게 함  

**디바운스(debounce)** : 짧은 시간 동안 동일한 이벤트가 많이 발생할 경우 이를 전부 처리하지 않고 처음 또는 마지막에 발생한 이벤트에 대해 한 번만 처리하는 것!  
⇒ 성능 최적화에 큰 도움을 주는 기능  

#### 5-3-4 커링 함수
**커링 함수(currying function)** : 여러 개의 인자를 받는 함수를 **하나의 인자만 받는 함수로 나눠서 순차적으로 호출**될 수 있게 체인 형태로 구성한 것  
```jsx
var curry3 = function (func) {
	return function (a) {
		return function (b) {
			return func(a, b);
		};
	};
};

var getMaxWith10 = curry3(Math.max)(10);
console.log(getMaxWiht10(8)); // 10
console.log(getMaxWiht10(25)); // 25
```
⇒ 인자 ↑ , 가독성 ↓ ⇒ **화살표 함수**를 써서 구현하는 것이 더 좋다!  

💡 **커링 함수**가 유용한 경우  
당장 필요한 정보만 받아서 전달하고 또 필요한 정보가 들어오면 전달하는 식으로 하면 결국 마지막 인자가 넘어갈 때까지 함수 실행을 미룸 ⇒ **지연실행**  
