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
    
