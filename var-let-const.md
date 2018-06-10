# var, let, const 기본 설명 및 차이점
항목 \ 선언자  |	 var	|	let	|	const
-|:-:|:-:|:-:
변수범위	|	`함수(function)`	|	`block {}`	| `block {}` 
재선언	|	O	|	**`X`**	|	**`X`**	
재할당	|	O	|	O	|	**`X*`**		
선언 할당	|	O	|	O	| **`O*`** 
호이스팅 사용	|	O	|	**`X*`**	|	**`X*`**	

- *const는 반드시 선언시 변수값을 할당하여 사용한다.

- *let, const는 호이스팅이 적용되지만, 호이스팅 후 변수의 값에 접근이 불가능하다. Reference Error 발생 - TDZ(Temporal Dead Zone).  이 때문에, **let과 const는 선언 할당 혹은 선언 후 할당의 순서를 지켜 사용하여야 한다.** let과 const는 이를 언어차원에서 강제하기 위해 ES6에서 추가되었다.    

- *코드/적용 범위 상단에 'use strict'를 추가하여 호이스팅 등을 금지할 수 있다.  ES5에서 추가.

  

    
## var 
- `함수(function)기준 범위`를 가진다. 따라서 `함수내에서 선언된 변수는 지역변수`가 되어 전역에서 사용할 수 없다.
- 변수 선언시 선언자 없이 선언할 경우, 전역 변수로 선언된다. 
- 함수내에서도 적용되므로, 함수내에서 선언자 없이 선언한 변수는 전역에서 사용 가능하다.
```javascript
i = 0;  // global variable
function fn() {
  k = 10;  // global variable
  console.log(i)  // 0
  var l = 20;  // local variable
}
console.log(i);  //  0
console.log(k);  //  10
console.log(l);  //  ReferenceError: l is not defined
```


- 선언자 없이 선언이 가능하지만, 선언하지 않은 변수의 값의 사용은 불가능하다.
```javascript
// i = 0;
console.log(i);  // ReferenceError: i is not defined
```



#### 예제1
```javascript
var i  =  0;
if( i > -1 ) {	
  var i = 10;
  var k = 20;
}
console.log(i);  //10
console.log(k);  //20
```
- var로 선언된 변수는 함수 기준 범위를 가지므로, if, for, while(블록 사용)은 **지역 변수를 가지지 못한다**. 그러므로 블록 내부에서 var로 선언된 변수는 전역 변수이다. (var의 변수는 재선언 가능)
- 지역변수 사용을 위해 예제2와 같이 `IIFE(Immediately Invoked Function Expression)`를 사용한다. 
- 다른 언어가 대부분 블록 기준의 변수 범위(scope)를 가지기 때문에 함수 기준 범위는 자바스크립트의 특성이자 혼란을 주는 요인이다.



#### 예제1: IIFE 적용
```javascript
var i  =  0;
if( i > -1 ) {
  (function(){ // IIFE
	  var i = 10;
  	  var k = 20;
  })();  // IIFE
}
console.log(i);  //  0
console.log(k);  //  ReferenceError: k is not defined
```
- 함수로 감싸진 내부의 변수는 지역 변수이다.



### 전역 변수, 지역 변수 (Globa, Local Variable)

- 함수내에서 전역변수과 같은 이름의 변수를 선언시, **지역 변수가 우선**시 된다.

#### 예제2
```javascript
var i = 0;
function  fn1() {
  var  i  =  10;
  console.log(i); // 10
}
function fn2() {
  console.log(i);  // undefined
  var i = 10
  console.log(i);  // 10
}
fn1();
fn2();
console.log(i); // 0
```
- fn1: 함수내에서 같은 이름의 변수가 선언 될시, **지역변수가 우선**시 된다.
- fn2: 함수내에서 전역변수를 먼저 사용후에 같은 이름의 지역변수 선언시, **호이스팅**과 **지역변수 우선**으로 위와 같은 결과가 나타난다.  
- 호이스팅에 의한 코드 순서 변화는 아래 참조.
- `호이스팅=선언 끌어올리기(Hoisting)`은 변수 선언이 함수나 전역부 내에서 최상단으로 이동하여 먼저 적용되는 것이다. 이로인해 변수를 먼저 사용하고 선언을 할 수있다. 



#### 예제2 : fn2 호이스팅 적용후
```javascript
function fn3() {
  var i = 10 // 호이스팅에 의해 선언부는 최상단으로 이동
  console.log(i);  // undefined  
  console.log(i);  // 10  
}
```



#### 예제3 : 함수내 전역변수 선언

```javascript
function fn() {
  i = 0;
}
function fn2() {
  var k = 10;
}
fn();
fn2();
console.log(i)  // 0;
console.log(k)  // RefferenceError: k is not defined;
```
- fn: 함수내에서 선언자 없이 선언한 변수는 전역변수이다. 
- fn2: 함수내에서 선언한 변수는 함수 외부에서 사용 할 수 없다. 






## let
- `블록 기준 단위의 범위(scope)`를 가진다.

```javascript
let i = 0;
if( i > -1 ) {
    let k = 10;
    console.log(i); // 0
}
console.log(k);  // RefferenceError: k is not defined;

```



- 재선언 불가

```javascript
let i;
let i;  // Uncaught SyntaxError: Identifier 'i' has already benn declared
```



- Hoisting 사용 불가능(Hoisting 되지만, 내부 값에 접근불가).    

```javascript
console.log(i);  //  undefined;
console.log(k));  //  RefferenceError: k is not defined;
var i;
let k;
```

  

- (브라우져에서) 전역 객체의 속성값(property)을 생성하지 않는다.

```javascript
var  i  =  0;
let  k  =  10;
console.log(this.i);  // 0 
console.log(this.k);  // undefined
console.log(k);  // 10
```



- 루프속의 이벤트/비동기(혹은 시간차가 발생하는) 조작 로직에서 var 선언 변수의 문제 해결: IIFE 사용하여 해결 가능.

```javascript
for( var i = 0; i < 3; ++i ) {
    setTimeout( function() { console.log(i) }, 500 );
}
console.log(i)  // 3
// 3
// 3
// 3

for( let k = 0; k < 3; ++k ) {
    setTimeout( function() { console.log(k) }, 500 );
}
console.log(k)  // RefferenceError: k is not defined;
// 0
// 1
// 2
```

- 블록내에서 var를 사용하여 선언하는 경우, 전역변수이다.  **내부 로직의 함수에서** 해당 변수를 선언 혹은 직접 넘겨받지 않고, **상위 범위의 변수를 바로 사용**한다.  (값의 복사가 아닌, **참조**.) 이미 사용이 끝나 스택에서 사라진 상위 범위의 변수를 참조 사용하는 내부 함수를 가리켜 `클로저-Closure` 라 한다. 
- 루프 구문이 끝난 후,  변수의 마지막 값을 참조하므로, 예상과는 다른 결과가 나온다.
- 블록안에서 let 키워드를 사용하여 선언한 변수는 각 블록 범위내에서 새로운 값을 가지게 된다. 



## const

- `블록 기준 단위의 범위(scope)`를 가진다.

```javascript
const COUNT = 0;
if( true ) {
    const NUM = 10;
}
console.log(COUNT);  // 0
console.log(NUM);  // RefferenceError: NUM is not defined
```



- `재선언/재할당/선언 후 할당 불가`

```javascript
const NUM = 0;
const NUM = 0;  // Uncaught SyntaxError: Identifier 'NUM' has already benn declared

NUM = 0;  // Uncaught TypeError: Assignment to constant variable

const COUNT;  // Uncaught TypeError: Missing initializer in const declaration
```



- 불변하는 `상수-constant`의 성격을 가진다. 하지만, 기본 자료형이 아닌, 오브젝트-참조 타입을 할당할 경우, 실제 값의 주소가 아닌 `객체의 주소를 변수에 저장`한다. 그러므로 불변하는 `상수는 참조 객체의 주소값`이며, 내부의 내용은 변경될 수 있다.

```javascript
const object = {};
object.new = "new";
console.log(object.new);  // new

const array = [1,2,3];
array[0] = 0;
console.log(array);  // [0,1,2]
```



- Hoisting 사용 불가능(Hoisting 되지만, 내부 값에 접근불가).    

```javascript
console.log(i);  //  undefined;
console.log(NUM));  //  RefferenceError: k is not defined;
var i;
const NUM = 0;
```

  

- (브라우져에서) 전역 객체의 속성값(property)을 생성하지 않는다.

```javascript
var  i  =  0;
const  NUM  =  10;
console.log(this.i);  // 0 
console.log(this.NUM);  // undefined
console.log(NUM);  // 10
```










> 참조(MDN Web docs 한/영)
>
> [var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var) 
>
> [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
>
> [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)