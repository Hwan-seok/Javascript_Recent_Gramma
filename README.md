**JS 최신 문법에 대해 "요약", "간단히" 작성할 예정입니다.
맞지 않거나 수정이 필요한 부분 언제든 말씀해주시면 감사합니다!**

# ECMAScript?
- ECMAScript란 javascript 사용이 많아지면서 생긴 ECMA 기구에서 만든 코어 스크립트 언어 (표준)


- 자바스크립트 자체를 개선하기 위하여 ECMA에서 2015년부터(ES6) 1년에 한번씩 변경되거나 추가된 기능들을 발표하기로 약속했습니다.
- 따라서 계속 ES6 7 8 9 를 붙히다가는 언제까지 갈지 모르므로 정식 명칭을 ES2015와 같이 년도를 붙히기로 했습니다 (현재까지 ES 2015 2016 2017 2018 까지 발표됨)
- 하지만 최신 문법들을 모든 브라우저에서 지원하는것이 아니며, chrome이 아니라면 발표되고 어느정도의 시간이 지나야 작동합니다.
- 또한 기본적으로 대부분의 최신 문법들은 예전 문법으로도 어떻게 이렇게 저렇게 하면 만들 수 있습니다.
- 그러므로 [babel](https://babeljs.io/)과 같은 trans-compiler를 사용하여 최신문법으로 작성된 코드를 예전 문법으로 바꾸어 사용한다면, 대부분의 브라우저에서도 작동합니다.

---
# ES2015 (ES6)
- ES6로 오면서 자바스크립트 언어 자체를 뒤흔들만한 기능들이 많이 나왔습니다.

## 변수 선언 방법 
### const, let
- const, let이라는 새로운 변수 선언 방법이 등장했습니다. 
```js
원래 방식 
var a = "Naver"
새로운 방식 
const a = "I want to"
let b = "join Naver"
```
- var로 선언한 변수는 **함수 스코프**를 가졌지만 const, let으로 선언한 변수들은 **블록 스코프**를 가집니다.
```js
{
    var a = "Really" 
    const a = "I want to"
    let b = "join Naver"
}
 console.log(a) // Really
 console.log(b) // Reference Err
 console.log(c) // Reference Err
```
- 근데 const에서 객체나 배열의 요소를 바꾸는건 가능 ...
```js
const c = [1, 2, 3];
c[0] = 4; //정상 처리
const d = {
    a:100
}
d.a=200 //정상
```
- 비동기 처리시의 예상치 못한 결과 완화 (블록 스코프)
for문 안에서 비동기 함수를 호출하면 콜백 함수가 호출되는 시점의 i값으로 계산
```js
for(var i = 0 ; i<5 ; i++){
  setTimeout(() => {
    console.log(i) //  4 4 4 4 4
  }, 100);
}
```
- let을 사용하니 예상대로 계산됨
```js
for(let i = 0 ; i<5 ; i++){
  setTimeout(() => {
    console.log(i) // 0 1 2 3 4
  }, 100);
}
```
왜그럴까..?
- 비동기적으로 for문이 실행되므로 ()=>{console.log(i)}가 5개 먼저 콜백큐에 들어가게 되는데 
이때 콜스택에서 실행될 시점에 var과 let의 컨텍스트 차이 때문에 다르게 동작한다.
* var는 실행될 시점의 i가 4이고, 모두 하나의 함수 안에 있으므로 4출력
* let은 각각 블록 안에서 i의 값이 모두 다르므로 실행될 시점의 i값이 같지 않다.

### 선언 메커니즘 변경
- var는 호이스팅이 발생해 모든 변수 선언이 위쪽으로 끌어올려져서 예상치못한 에러가 발생했었습니다.
- let, const 는 조금 다른 호이스팅이 발생하며, var과 달리 선언, 초기화, 할당이 모두 분리되어있어서 실제 코드상으로 선언되는 부분 이전에 접근하면 오류를 발생합니다.
* 변수는 다음과같이 3단계로 선언됩니다.
  * 선언 -> 나 이제 이런 변수를 쓸꺼야!
  * 초기화 -> 이 변수는 메모리 이부분부터 쓸꺼야
  * 할당 -> 이 메모리에 이 값을 저장할꺼야
- var 로 선언한 변수는 선언과 초기화가 함께 이루어짐
![image](https://media.oss.navercorp.com/user/12640/files/9f9d70d2-2ec0-11e9-9959-e0b1d145bf9f)

- let, const로 선언한 변수는 선언만하고 초기화와 할당은 나중에 이루어짐
![image](https://media.oss.navercorp.com/user/12640/files/949fae52-2ec0-11e9-979a-a47fb5b2aac5)
-> 호이스팅의 변화가 생김
```js
console.log(a) // undefined
var a
console.log(b) // Reference Err  
let b
```


## object 
- 객체를 만들 때에 key에 대입되는 value의 이름이 같을때 생략이 가능해졌습니다.

```js
const name = "naver"
const age = 3
const ob = {
  name : name,
  age : age
```
이처럼 중복적으로 같은 문자를 적어줬지만, key value이름이 같으면 다음과 같이 편리하게 
```js
const name = "naver"
const age = 3
const ob = {
  name,
  age
```
## 함수
-  함수를 표현하는 방법이 추가되었습니다.

### 화살표 함수

```js
var a = function(q,w){return q+w} // 원래 방법

let b = (q,w)=>{return q+w} // 추가된 방법1
let c = (q) => q*2 //방법 2 
let d = q => q*2 //방법 3
```
- 화살표 함수는 일반 함수와 다르게 this가 변하지 않습니다.
- foreach문 안에서는 this가 변하지만 화살표함수로 foreach를 열어주면 this가 바인딩됩니다.

```js
let root = {
  name:"Naver",
  array:[1,2,3],
  ArrowFunc(){
    this.array.forEach(element => {
    console.log(this.name) // root객체의 this
    })},

  NormalFunc(){
  self=this
  this.array.forEach(function(){
    console.log(this.name) // foreach의 this
    console.log(self.name) // root 객체의 this
  })},
}
```
### rest (나머지)
- ...argument 부분에 들어온 인자를 배열로 만들어줍니다.
- 함수에 몇개의 인자가 들어올지 모르는 상에서 유용합니다.

```js
const func4 = (x, ...argument) => {
    argument.foreach(element=>{
        x+=argument
    })
    return x
};
func4(1, 2, 3, 4) // 10
```

### spread 
- rest는 여러 인자를 배열로 만들어주는 거라면, spread는 배열(객체)을 여러 인자로 만들어줍니다.
```js
let array = [1, 2, 3];
let func = function(x, y, z) {
  alert(x + y + z);
};
func(array[0], array[1], array[2]); // 6
func(...array) // 6
```


 ## 문자열
 
 ### 템플릿 리터럴
 - grave accent ( \` ) 를 사용하여 좀 더 직관적인 문자열을 만들 수 있습니다.
 
 ```js
 const q = "name"
 const w = "kang"
 
 const e = "my first " + q + "is " + w
 const r = `my first ${q} is ${w}`
```

## 클래스

- 다른 객체지향 언어에서 주로 쓰이는 class들과 거의 동일하지만, 내부적으로는 아직 prototype으로 동작되고 있습니다.

```js
class Human {
  constructor(type = 'human') {
    this.type = type;
  }

  static isHuman(human) {
    return human instanceof Human;
  }

  breathe() {
    alert('h-a-a-a-m');
  }
}

class Zero extends Human {
  constructor(type, firstName, lastName) {
    super(type);
    this.firstName = firstName;
    this.lastName = lastName;
  }

  sayName() {
    super.breathe();
    alert(`${this.firstName} ${this.lastName}`);
  }
}
const newZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(newZero); // true
```

## destructuring (비 구조화)

- spread, rest와 궁합이 잘 맞는 기능으로, 객체나 배열을 찢어서 쓰기 좋게 할당해줍니다.
- 일일이 찾아서 넣어줄 필요 없이 대충 해도 알아서 들어간다!

```js
const [c, ,d] = [1, 2, 3];
console.log(c); // 1
console.log(d); // 3
```
```js
let ob = {
    i:"i",
    k:{
        j:"k"
    }
}
//원래 
let i=ob.i  
let k=ob.k
let j=ob.k.j
// ES6
let {i,k,j,m}=ob 
```

## promise

- ES6 문법에서 let, const와 함께 가장 중요하다고 생각되는 문법
- 자바스크립트에서 흔히 볼 수 있는 콜백 헬을 조금이나마 완화할 수 있습니다.



### promise를 작성하는 방법
- 함수 시작부에 바로  `return new Promise((resolve,reject)=>`로 프로미스를 반환한다는 표시를 해줍니다.
- 함수의 목적에 맞게 작성하다가, 성공적으로 리턴해야할 부분에 `resolve(result)`함수에 결과를 담아 호출하고, 실패할때 `reject(result)` 함수를 호출합니다.
```js
const 어떤_함수 = ()=>{
    return new Promise((resolve,reject)=>{
        try{
            어떤일하다가
            resolve("성공!")
        }catch(e){
            reject("실패!")
        }
    })
}
```

### promise를 반환하는 함수를 사용하는 방법
* 성공 (resolve)
  * `.then`으로 이동하여 아까 resolve(결과)함수로 넘긴 결과를 가져옵니다.
* 실패 (reject)
  * `.catch`로 이동하여 reject(결과)함수로 넘겨진 결과를 가져옵니다.
  
```js
어떤_함수()
.then((result) => {
    console.log(result) //성공!
})
.catch((err)=>{
    console.log(err) //실패!
})

```

- `.then`에서 promise를 반환하는 함수를 다시 리턴하면 .then을 이어쓸 수 있습니다.

```js
Users.findOne({})
.then((user) => {
    user.name = 'bada';
    return user.save();
  }).then((user) => {
    return Users.findOne({ gender: 'm' });
  }).then((user) => {
    // ...
  }).catch(err => {
    console.error(err);
  });
```

## 모듈 관리
- import 방식이 추가되었다
- reqiure("모듈명") -> import a form "모둘명" 
- Javascript는 java와 달리 export를 해줘야 import를 할 수 있습니다.

### import

1. default export만 가져옵니다
2. "name"으로 alias를 설정하여 가져옵니다. (name.어떤것)
3. member로 export된것만 가져옵니다
4. member로 export된것을 alias로 이름을 바꿔서 가져옵니다
5. member1,2를 가져옵니다 
/////

```js
import name form "module-name";                        //1
import * as name from "module-name";                 //2
import { member } from "module-name";               //3 
import { member as alias } from "module-name"; //4 
import { member1, member2 } from "module-name"; //5
import { member1, member2 as alias2, [...] } from "module-name"; //6
import defaultMember, { member [, [...]] } from "module-name";//7
import defaultMember, * as alias from "module-name";//8
import defaultMember from "module-name";//9

```

### export
- default export는 하나의 파일당 하나밖에 할 수 없으므로 메인 클래스나 함수를 하는것이 좋습니다.
- export를 하지 않으면 import를 할 수 없습니다.

```js
export { name1, name2, ..., nameN };
export { variable1 as name1, variable2 as name2, ..., nameN };
export let name1, name2, ..., nameN; // 또는 var
export let name1 = ..., name2 = ..., ..., nameN; // 또는 var, const
 
export expression;
export dafault expression;
export default function (...) { ... } // 또는 class, function*
export default function name1(...) { ... } // 또는 class, function*
export { name1 as default, ... };

```


# ES2016 (ES7)

- ES2015때 너무 많이 추가되서인지 이때 추가된 기능이 거의 없습니다.

# ES2017 (ES8)
- 마찬가지로 추가된 기능이 많이 없지만, 엄청난 기능이 하나 추가되었습니다.

## async / await
- ES6때 콜백 헬 -> promise로 완화하는 과정을 거쳤습니다. 하지만, 개발자의 욕심은 끝이 없어서 더 깔끔한 코드를 보기 원하는 사람들이 이 기능을 만들어 냅니다.
- 이전에 소개하지 않았지만 원래 generator라고 async/await와 동일한 기능을 하는 문법이 있었으나, 이 문법보다 어렵기도 하고 완벽히 대체되었기 때문에 소개하지 않았습니다.


- promise로 작성된 몽구스 연동 코드를 보시면, 이것만으로도 어느정도 깔끔하다고 생각할 수도 있습니다. 하지만, 살짝 어지럽다고 생각될수도 있습니다.
```js
  //            promise 예제
  Users.findOne({ name: 'Naver' }) 
  .then((result) => { //result에는 name의 value가 Naver인 도큐먼트가 있음
    return Users.update({ name: `join ${result.name}` }, { updated: true }); 
  })
  .then((updatedResult) => {
    console.log(updatedResult);
  })
  .catch((err) => {
    console.error(err);
  });
  ```
  
  
- async / await로 작성된 코드를 보시면, 위 코드와 비교했을때 훨신 가독성도 높고 깔끔하다는것을 볼 수 있습니다.  위의 코드와 정확히 동일하게 동작합니다.

```js
  //            async / await 예제
async ()=>{
  try{
    const result = await Users.findOne({ name: 'Naver' }) 
    const updatedResult = await User.update({ name: `join ${result.name}` }, { updated: true })
    console.log(updatedResult)
  }catch(err){
    console.log(err)
  }
}
```
  
### async / await사용법
- 비동기적으로 실행되야 하는 함수는 꼭 promise를 반환해야합니다.
- 비동기로 실행되는 함수의 앞에 async를 붙혀주고, 그 함수 내에서 콜백과 같이 처리되야 할때 앞에 await를 붙혀줍니다. 

```js
const a =  (arg)=>{ //콜백으로 실행되야하는 함수 
    return new Promise((resolve,reject)=>{ //프로미스 반환
        if(arg==="몽고디비에서 뭐좀 가져와"){
		// 여기서 db task
            resolve("이런거 가져왔어")
        }else{
            reject("다른거 할게")
        }
    })
}

const main= async () => { // 비동기 함수앞에 async 붙여줌
console.log(await a("몽고디비에서 뭐좀 가져와"))  // 동기로 실행되야하면 앞에 await 부착
}

main() 
```

# ES2018 (ES9)
- 상당하게 추가된건 없고, 기존에 있던 promise, async, await기능이 강화됨

# ES NEXT
- ES NEXT는 다음 업데이트에 포함될 기능들을 선정하는 단계같은것입니다.
- ES NEXT는 stage 0~4까지 있으며 stage-4는 Finished(종료), stage-3는 Candidate(후보), stage-2는 Draft(초안), stage-1은 Proposal(제안), stage-0은 Strawman(허수아비)입니다.
- 현재 stage 3에 클래스 접근 제한자 private이 있어서 다음(ES2019) 업데이트때 포함될 확률이 매우 높습니다. 점점 객체지향의 ...

참고한 문서
 [호이스팅, 스코프]( https://poiemaweb.com/es6-block-scope)
[ zero cho님 블로그](https://www.zerocho.com/category/ECMAScript/post/57d51fd976f9ec00153633b5 " zero cho님 블로그")
