# 02. 실행 컨텍스트  

## 2-1 실행 컨텍스트란?
**실행 컨텍스트** : 실행할 코드에 제공할 환경 정보들을 모아놓은 객체  

#### 💡 스택(stack)과 큐(queue)

##### 스택(stack)
: 출입구가 하나뿐인 깊은 우물 같은 데이터 구조

- 용량보다 많은 데이터를 저장하려고 하면 넘친다 → **stack overflow**

##### 큐 (queue)
: 양쪽이 모두 열려 있는 파이프 같은 데이터 구조

<img width="443" alt="스크린샷 2023-03-07 오후 1 47 03" src="https://user-images.githubusercontent.com/101851472/223404773-d5590b33-8940-4d2e-8fc4-57200b4c27fe.png">

동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 컨텍스트를 구성하고, 이를 콜 스택(call stack)에 쌓아 올렸다가, 가장 위에 쌓여있는 컨텍스트와 관련 있는 코드들을 실행하는 방식으로 전체 코드의 환경과 순서를 보장!

→ 실행 컨텍스트를 구성하는 방법 : **함수 실행**
~~~javaScript
var a = 1;
function outer() {
	function inner() {
		console.log(a); //undefined
		var a = 3;
	}
	inner();
	console.log(a); // 1
}

outer();
console.log(a); // 1
~~~
<img width="483" alt="스크린샷 2023-03-07 오후 3 39 40" src="https://user-images.githubusercontent.com/101851472/223405318-da08a6d8-7b3f-4780-900a-67b4f59804ad.png">

(**전역 컨텍스트** : 자바스크립트 파일이 열리는 순간 자동으로 실행)

어떤 실행 컨텍스트가 활성화될 때 자바스크립트 엔진은 해당 컨텍스트에 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 **실행 컨텍스트 객체**에 저장합니다. 이 객체는 자바스크립트 엔진이 활용할 목적으로 생성! 

- **담기는 정보들**
    - **VariableEnvironment** : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보, 선언 시점의 LexicalEnvironment의 **스냅샷**으로, **변경 사항은 반영되지 않음**.
        - 스냅샷 → 특정 시점!
    - **LexicalEnvironment** : 처음에는 VariableEnvironment와 같지만 **변경 사항이 실시간으로 반영**됨.
    - **ThisBinding** : **this 식별자**가 바라봐야 할 대상 객체.
    
<img width="449" alt="스크린샷 2023-03-07 오후 5 43 12" src="https://user-images.githubusercontent.com/101851472/223682277-cfa9376a-1f78-4140-999d-a83993877cdf.png">

## 2-2 VariableEnvironment

- VariableEnvironment에 정보를 담음 → 이를 그대로 복사해서 LexicalEnvironment를 만듬 → **LexicalEnvironment**를 주로 활용

## 2-3 LexicalEnvironment  
### 2-3-1 environmentRecord와 호이스팅
**environmentRecord** : 현재 컨텍스트와 관련된 코드의 **식별자 정보**들이 저장 (순서대로 수집)

자바스크립트 엔진은 식별자들을 최상단으로 끌어올려놓은 다음 실제 코드를 실행한다! (환경에 속한 코드의 변수명들을 올린 후 코드 실행)

**호이스팅(hoisting)** : 변수 정보를 수집하는 과정을 더욱 이해하기 쉬운 방법으로 대체한 가상의 개념 (편의상 끌어올린 것(수집한 것)으로 간주하자는 것)

- 호이스팅 규칙

> 매개변수와 변수에 대한 호이스팅(1) - 원본 코드
```jsx
function a (x) {
	console.log(x); // (1) 예상 : 1
	var x;
	console.log(x); // (2) 예상 : undefined
	var x = 2;
	console.log(x); // (3) 예상 : 2
}
```

> 매개변수와 변수에 대한 호이스팅(2) - 매개변수를 변수 선언/할당과 같다고 간주해서 변환한 상태
> 

```jsx
function a () {
	var x = 1;
	console.log(x); // (1)
	var x;
	console.log(x); // (2)
	var x = 2;
	console.log(x); // (3)
}
```

→ **호이스팅 처리**!! (변수명만 끌어올리고 할당 과정은 원래 자리에 그래도 남겨둡니다)

> 매개변수와 변수에 대한 호이스팅(3) - 호이스팅을 마친 상태
> 

```jsx
function a () {
	var x; // 변수 x 선언 → 메모리에서 저장 공간 확보, 주솟값을 변수 x에 연결
	var x; // 다시 변수 x 선언 
	var x; // 무시

	x = 1; // x에 1을 할당 → 1을 별도의 메모리에 담고, 주솟값 입력
	console.log(x); // 1
	console.log(x); // 1
	x = 2; // x에 2를 할당 → 2을 별도의 메모리에 담고, 1을 가리키는 주솟값을 2를 가리키는 주소값으로 대치
	console.log(x); // 2
}

a(1);
```

- **변수**는 선언부와 할당부를 나누어 **선언부**만 끌어올림
- **함수** 선언(문)은 **함수 전체**를 끌어올림

### 📍함수 선언문과 함수 표현식

: 함수를 새롭게 정의할 때 사용하는 방식

- **함수 선언문** : function 정의부만 존재하고 별도의 할당 명령이 없는 것 (반드시 **함수명** 정의)
- **함수 표현식** : 정의한 function을 별도의 변수에 할당하는 것
	- **기명 함수 표현식** : 함수명을 정의한 함수 표현식
	- **익명 함수 표현식** : 함수명을 정의하지 않은 표현식

```jsx
function a () {} // 함수 선언문
a(); // 실행 o

var b = function () {} // (익명) 함수 표현식
b(); // // 실행 o

var c = function d () {} // 기명 함수 표현식
c(); // 실행 o
d(); // error (함수명은 오직 내부에서만 접근 가능)
```

**호이스팅 시 차이점**
- **함수 선언문** : **전체**를 호이스팅
- **함수 표현식** : 변수는 **선언부**만 끌어올립니다. (할당부 이후부터 실행 가능)

#### 2-3-2 스코프, 스코프 체인, outerEnvironmentReference
- **스코프** : 식별자에 대한 유효범위  
- **스코프 체인** : 스코프를 안에서부터 바깥으로 차례로 검색해 나가는 것

→  LexicalEnvironment의 **outerEnvironmentReference**로 인해 가능!  

##### **스코프 체인**  
outerEnvironmentReference : 현재 호출된 함수가 **선언될 당시**의 LexicalEnvironment를 참조 (과거 시점) / **연결 리스트 형태**  
→ **선언**하는 행위 : 콜 스택 상에서 **어떤 실행 컨텍스트가 활성화된 상태**일 때뿐!!!  

여러 스코프에서 동일한 식별자를 선언한 경우에는 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능

```jsx
var a=1;
var outer = function(){
	var inner = function(){
		console.log(a); //undefined
		var a = 3;
	};
	inner();
	console.log(a); // 1
};
outer();
console.log(a); // 1
```

L.E(LexicalEnvironment) / e(environmentRecord) / o(outerEnvironmentReference) / [숫자] (코드 줄 번호)
<img width="398" alt="스크린샷 2023-03-09 오전 1 30 12" src="https://user-images.githubusercontent.com/101851472/223777396-7c7b699c-2d9b-4911-83bf-0acac46a8b6d.png">

전역 컨텍스트 → outer 컨텍스트 → inner 컨텍스트 
- 점차 규모 작아짐
- 스코프 체인을 타고 접근 가능한 변수의 수 늘어남

**변수 은닉화** : inner 함수 내부에서 a 변수를 선언했기 때문에 전역 공간에서 선언한 동일한 이름의 a 변수에는 접근할 수 없음

##### **전연변수와 지역변수**
- 전역변수 (a, outer)
- 지역변수 (inner, a)

## 2-4 this
thisBinding에는 this로 지정된 객체가 저장됨 → this가 지정되지 않은 경우 전역 객체가 저장됨
