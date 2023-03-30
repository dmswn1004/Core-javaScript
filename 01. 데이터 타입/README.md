# 01. 데이터 타입  

## 1-1 데이터 타입의 종류  
> 기본형(primitive type) : 숫자, 문자열, 불리언, null, undefined + Symbol  
> 참조형(reference type) : 객체, 배열, 함수, 날짜, 정규 표현식 + Map, weakMap, Set, WeakSet  

<img width="337" alt="스크린샷 2023-03-02 오전 2 47 19" src="https://user-images.githubusercontent.com/101851472/222220561-73556082-0f8d-43ae-ab73-67672f439add.png">

- 기본형과 참조형을 구분하는 기준?
  - 기본형 : 값이 담긴 주솟값을 바로 복제 
  - 참조형 : 값이 담긴 주솟값들로 이루어진 묶음을 가리키는 주솟값을 복제  

기본형은 **불변성**을 뛴다!  

## 1-2 데이터 타입에 관한 배경지식  
#### 1-2-1 메모리와 데이터  
- 메모리 → 매우 많은 비트(0, 1)들로 구성
  - 각 비트는 **고유한 식별자**를 통해 위치 확인
  → 바이트(1byte = 8bit)
  
**메모리 주솟값**을 통해 서로 구분하고 연결!!  

#### 1-2-2 식별자와 변수  
- 변수 : 변할 수 있는 데이터
- 식별자(변수명) : 어떤 데이터를 식별하는 이름  

## 1-3 변수 선언과 데이터 할당  
#### 1-3-1 변수 선언  
~~~javaScript
var a; // 변수 a 선언
~~~  
변수 선언 : 변경 가능한 데이터가 담길 수 있는 공간  
- 과정 : 비어있는 공간 하나를 확보 → 식별자 a라고 지정  
<img width="400" alt="스크린샷 2023-03-04 오후 7 07 14" src="https://user-images.githubusercontent.com/101851472/222893915-16bec9fd-d3d9-4ea0-8106-b5b87ef46eca.png">

#### 1-3-2 데이터 할당  
~~~javaScript
a = 'abc'; // 변수 a에 데이터 할당

var a = 'abc'; // 변수 선언과 할당을 한 문장으로 표현
~~~  
- 과정 : a라는 이름을 가진 주소 검색 → 검색한 곳에 문자열 'abc'를 할당
▶︎ 실제 : 데이터를 저장하기 위한 별도의 메모리 공간을 다시 확보해서 문자열 저장 후, 그 주소를 변수 영역에 저장 → **변수 영역, 데이터 영역**
<img width="400" alt="스크린샷 2023-03-04 오후 7 06 29" src="https://user-images.githubusercontent.com/101851472/222893890-7e8e5efb-dc1a-433d-8327-a2ce663a94ef.png">

변수 영역에 값을 직접 대입하지 않는 이유?  
  - 데이터 변환을 자유롭게 함  
  - 메모리를 더욱 효율적으로 관리  
  
## 1-4 기본형 데이터와 참조형 데이터  
#### 1-4-1 불변값  
변수, 상수 → **변경 가능성**으로 구분(대상 : 변수 영역 메모리 ▶︎ 재할당 여부가 관건)  
불변값, 상수? (대상 : 데이터 영역 메모리)  
- 불변성 : 한 번 만든 값을 바꿀 수 없고, 변경은 새로 만드는 동작을 통해서만 이뤄진다.  
cf) 기본형 데이터는 모두 불변값!

#### 1-4-2 가변값  
> 참조형 데이터의 할당  
~~~javaScript
var obj1 = {
  a: 1,
  b: 'bbb'
};
~~~
<img width="401" alt="스크린샷 2023-03-04 오후 7 42 29" src="https://user-images.githubusercontent.com/101851472/222895508-4b25689f-f572-4b43-ae50-2867e2be5895.png">

- 과정 : 변수 영역의 빈 공간 확보 & 이름 obj1로 지정 → 데이터 영역 생성(그룹 내부의 프로퍼티들을 저장하기 위해 별도 변수 영역 마련 후 주소 저장) → 별도 변수 영역에 각각 a, b로 이름 지정 → 데이터 저장을 위해 검색 후 저장 공간 생성  

기본형 데이터와의 차이는 객체의 **변수(프로퍼티) 영역**이 별도로 존재한다는 점!  
→ 변수에는 다른 값을 얼마든지 대입 가능!  
> 참조형 데이터의 프로퍼티 재할당
~~~javaScript
obj1.a = 2;
~~~
변경 과정 : 데이터 영역에서 숫자 2 검색 후 없으므로 빈 공간에 저장 → 변수 영역의 주솟값 변경  
▶︎ 새로운 객체가 만들어진 것이 아니라 기존의 객체 내부의 값만 바뀐 것!  
<img width="399" alt="스크린샷 2023-03-06 오전 2 30 09" src="https://user-images.githubusercontent.com/101851472/222976183-cdce1722-8b43-4493-adf9-aab4a8f7cba7.png">

<br>

- **중첩 객체** : 참조형 데이터의 프로퍼티에 다시 참조형 데이터를 할당하는 경우  
~~~javaScript
var obj = {
  x: 3,
  arr: [3, 4, 5] 
};
~~~
<img width="399" alt="스크린샷 2023-03-06 오전 2 33 11" src="https://user-images.githubusercontent.com/101851472/222976320-fad94253-3fef-45b9-9cbc-273776cf7e03.png">

- 문자열 재할당 후 사용하지 않는 데이터는 참조 카운트가 0이 된다. 참조 카운트가 0인 메모리 주소는 **가비지 컬렉터**의 수거 대상이 된다.
- 가비지 컬렉터는 특정 시점이나 메모리 사용량이 포화 상태에 임박할 때마다 자동으로 수거 대상들을 수거하고, 수거된 메모리는 다시 새로운 값을 할당할 수 있는 빈 공간이 된다.

#### 1-4-3 변수 복사 비교  
(기본형 데이터 vs 참조형 데이터)  
> 변수 복사  
~~~javaScript
var a = 10; // 기본형 데이터
var b = a;

var obj1 = {c: 10, d: 'ddd'}; // 참조형 데이터
var obj2 = obj1;
~~~
<img width="399" alt="스크린샷 2023-03-06 오전 2 47 18" src="https://user-images.githubusercontent.com/101851472/222977018-9ff6a1cf-3424-4273-ab66-ad7574da5ba5.png">

▶︎ 변수 복사하는 과정은 모두 같은 주소를 바라보게 되는 점에서 동일!  
<br>  
  
> 변수 복사 이후 값 변경 결과 비교 - 객체의 프로퍼티 변경 시  
~~~javaScript
b = 15;
obj2.c = 20;
~~~
<img width="399" alt="스크린샷 2023-03-06 오전 3 00 56" src="https://user-images.githubusercontent.com/101851472/222977605-f40307ed-5884-4907-a048-e5ee31f5ff28.png">

▶ 기본형 데이터 : 값이 변경됨, 참조형 데이터 : 변경 x  
<br>
  
> 변수 복사 이후 값 변경 결과 비교 - 객체 자체를 변경했을 때  
~~~javaScript
b = 15;
obj2.c = {c: 20, d: 'ddd'};
~~~
<img width="399" alt="스크린샷 2023-03-06 오전 3 11 21" src="https://user-images.githubusercontent.com/101851472/222978060-87c64d99-146c-4565-ad62-0af893cb2f75.png">

## 1-5 불변 객체  
#### 1-5-1 불변 객체를 만드는 간단한 방법  
- 기존 데이터는 변하지 않고 내부 프로퍼티를 변경할 필요가 있을 때마다 매번 새로운 객체를 만들어 재할당하기로 규칙을 정하거나 자동으로 새로운 객체를 만드는 도구 활용하면 불변성 확보 가능  
**불변 객체** → 값으로 전달받은 객체에 변경을 가하더라도 원본 객체는 변하지 않아야 하는 경우 필요!  

> 기존 정보를 복사해서 새로운 객체를 반환하는 함수(얕은 복사) copyObject를 이용한 객체 복사
~~~javaScript
var user = {
  name: 'Jaenam',
  gender: 'male'
};

var copyObject = function(target) {
  var result = {};
  for (var prop in target) {
    result[prop] = target[prop];
  }
  return result;
}

var user2 = copyObject(user);
user2.name = 'Jung';

if (user1= user2) {
  console.log('유저 정보가 변경돠었습니다.'); // 결과 : 유저 정보가 변경돠었습니다.
}

console.log(user.name, user2.name); // Jaenam Jung
console.log(user == user2); // false
~~~

▶︎ user 객체 내부의 변경이 필요할 때 무조건 copyObject 함수를 사용하기로 합의하고 그 규칙을 지킨다는 전제하에서 user 객체가 **불변 객체**라고 볼 수 있다.  

#### 1-5-2 얕은 복사와 깊은 복사  
- 얕은 복사 : 바로 아래 단계의 값만 복사하는 방법 → 주소값만 복사한다는 의미!  
- 깊은 복사 : 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법  
> 중첩된 객체에 대한 얕은 복사  
~~~javaScript
var user = {
  name: 'Jaenam',
  urls: {
    portfolio: 'http://github.com/abc',
    blog: 'http://blog.com/abc',
    facebook: 'http://facebook.com/abc'
  }
};
var user2 = copyObject(user);

user2.name = 'Jung';
console.log(user.name === user2.name); // false

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio); // true

user.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog); // true
~~~
▶︎ user 객체에 직접 속한 프로퍼티에 대해서는 복사해서 완전히 새로운 데이터가 만들어진 반면, 한 단계 더 들어간 urls의 내부 프로퍼티들은 기존 데이터를 그대로 참조  

> 중첩된 객체에 대한 깊은 복사  
~~~javaScript
var user2 = copyObject(user);
user2.urls = copyObject(user.urls);

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio); // false

user.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog); // false
~~~
▶︎ 객체를 복사할 때 객 내부의 모든 값을 복사해서 완전히 새로운 데이터를 만들고자 할 때, **기본형 데이터**일 경우에는 그대로 복사하면 되지만 **참조형 데이터**는 다시 그 내부의 프로퍼티들을 복사해야 함

→ 참조형 데이터가 있을 때마다 **재귀적**으로 함수를 호출하여 객체를 완전히 복사해야 한다!

> JSON을 활용한 간단한 깊은 복사 (메서드나 숨겨진 프로퍼티인 __proto__나 getter/setter등과 같이 JSON으로 변경할 수 없는 프로퍼티들은 모두 무시)  
~~~javaScript
var copyObjectViaJSON = function (target) {
  return JSON.parse(JSON.stringify(target));
};
var obj = {
  a: 1,
  b: {
    c: null,
    d: [1, 2],
    func1: function () {console.log(3);}
  },
  func2: function () {console.log(4);}
};
var obj2 = copyObjectViaJSON(obj);

obj.a = 3;
obj.b.c = 4;
obj.b.d[1] = 3;

console.log(obj); // {a: 1, b: {c: null, d: [1,3], func1: f() }, func2: f() }
console.log(obj2); // {a: 3, b: {c: 4, d: [1,2]}
~~~

## 1-6 undefined와 null  
- **undefined**  
  1. 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때  
  2. 객체 내부의 존재하지 않 프로퍼티에 접근하려고 할 때  
  3. return 문이 없거나 호출되지 않는 함수의 실행 결과  
  
→ 그 자체로 값, 비어 있음  
> undefined와 null의 비교  
~~~javaScript
var n = null;
console.log(typeof n); // object

console.log(n == undefined); // ture
console.log(n == null); // ture


console.log(n === undefined); // false
console.log(n === null); // ture
~~~
