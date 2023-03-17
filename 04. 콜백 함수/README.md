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
