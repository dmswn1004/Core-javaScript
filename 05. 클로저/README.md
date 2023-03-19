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

