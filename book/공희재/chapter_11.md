# Chapter 11 - 원시 값과 객체의 비교

앞서 6장에서 살펴봤듯 자바스크립트의 8가지 데이터 타입들은 크게 원시 타입(**primitive type**) 객체 타입(**object/reference type**)으로 나뉜다.

원시 타입과 객체 타입은 세 가지 측면에서 다르다.
* **원시** 값은 **immutable value**(변경 불가능한 값)이고, **객체** 값은 **mutable value**(변경 가능한 값)이다.
* **원시** 값을 변수에 할당할 땐 **실제 값**이 저장되고, **객체** 값을 변수에 할당할 땐 **참조 값**이 저장된다.
* **원시** 값을 가진 변수를 다른 변수에 할당하면 **원본의 원시 값이 복사되어 전달**되고(=**pass by value**), **객체** 값을 가진 변수를 다른 변수에 할당하면 **원본의 참조 값이 복사되어 전달**된다(=**pass by reference**).
<br>

## 1. Primitive: 원시 값
### 1.1. 변경 불가능한 값
원시 값은 변경 불가능한 값, 즉 읽기 전용 값이다. 원시 값은 어떤 일이 있어도 변하지 않는다.<br>
이러한 원시 값의 불변성은 데이터의 신뢰성을 보장한다.

<img src="https://github.com/user-attachments/assets/4da8d5fc-315a-45e4-a7b6-2a28e126805c" width="700px" />

변수에 할당된 원시 값을 바꾸고 싶다면,<br>
새로운 메모리 공간을 확보해 재할당하고자 하는 값을 저장 후, 변수가 참조하던 메모리 공간의 주소를 변경해야 한다.<br>
즉, 엄밀히 말하자면 저장된 값을 **교체하는 방법 외에는 없다**. 이것이 원시 값의 **불변성**을 설명하는 대표적인 특징이다.
<br><br>

### 1.2. 문자열과 불변성
자바스크립트는 특이하게도 다른 언어들과 달리 문자열을 원시 값으로 관리한다.<br>
따라서 변수에 저장된 문자열을 변경하고 싶다면, 사실상 변경이 아닌 교체를 할 수밖에 없다.<br>
**문자열은 원시 값이고, 원시 값은 변경 불가능한 값이기 때문이다.**

그러나 자바스크립트에서 문자열은 **유사 배열 객체이면서 이터러블**이므로,<br>
다른 언어들이 배열을 취급하는 것과 유사하게 아래처럼 각 문자에 접근도 가능하다.<br>
단, 접근만 가능할 뿐, 접근한 문자를 변경하는 등의 동작은 하지 않는다는 걸 주의하자.
```javascript
var str = 'string';

// 문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근 가능.
console.log(str[0]);

// 원시 값인 문자열이 객체처럼 동작하는 걸 볼 수 있음.
console.log(str.length);  // 6
console.log(str.toUpperCase());  // STRING

// 그러나! 문자열은 원시 값이므로 변경할 수 없다. 따라서 다음은 작동하지 않는다.(게다가 에러도 발생하지 않는다.)
str[0] = 'S';
console.log(str);  // string
```
<br>

### 1.3. Pass By Value: 값에 의한 전달
다음 예제에서 알 수 있듯, 원시 값을 갖는 변수를 다른 변수에 할당하면,<br>
할당받는 변수(`copy`)에는 할당하는 변수(`score`)의 **원시 값이 복사돼** 전달된다는 사실을 기억하자.

```javascript
var score = 80;
var copy = score;

console.log(score, copy);  // 80 80

score = 100;

console.log(score, copy);  // 100 80
```

<img src="https://github.com/user-attachments/assets/8336921e-30fb-48cb-8d96-e4a7e297030b" width="700px" />
<br>

## 2. Object: 객체
객체는 프로퍼티의 개수가 정해져 있지 않으며, 동적으로 추가/삭제될 수 있다.<br>
게다가 프로퍼티의 값에도 제약이 없어 값이 무엇이든 될 수 있다.<br>
이토록 **객체는 복합적이고 비용이 많이 드는 자료구조**이므로 원시 값과는 다른 방식으로 동작하도록 설계되어 있다.

### 2.1. 변경 가능한 값
객체(참조) 타입의 값은 변경 가능한 값이다. 일단 변수에 객체를 할당하면 무슨 일이 일어나는지 알아보자.
```javascript
var person = {
  name : 'Lee'
};
```
<img src="https://github.com/user-attachments/assets/cdf3167c-7140-4bd5-ac90-8384ec9afdb8" width="300px" />

객체를 할당한 변수를 참조하면 메모리에 저장되어 있는 **참조 값**을 통해 실제 객체에 접근한다.<br>
즉, `person`이라는 변수는 객체 `{ name:'Lee' }` 값을 갖는 게 아니라, 그 값을 **가리키고(참조하고) 있다**고 표현해야 한다.

원시 값을 취급하던 방식과는 다르게, 객체를 할당한 변수는 재할당 없이 **메모리에 저장된 객체를 직접 변경할 수 있다**.<br>
아래처럼 프로퍼티를 동적으로 추가할 수도 있고, 프로퍼티 값을 갱신할 수도 있으며, 프로퍼티 자체를 삭제할 수도 있다.<br>
이때 객체를 할당한 변수 `person`에 **재할당을 하지 않으므로, 객체를 할당한 그 참조 값은 변경되지 않는다.**

```javascript
//프로퍼티 값 갱신
person.name = 'Kim';

//프로퍼티 동적 생성
person.address = 'Seoul';
```
<img src="https://github.com/user-attachments/assets/9e5586a6-9af1-43cb-baea-95271d5a7b18" width="300px" />
<br><br>

### 2.2. Pass By Reference: 참조에 의한 전달
객체의 이러한 작동 방식에 따른 **부작용**이 있다. 바로 **여러 개의 식별자가 하나의 객체를 공유할 수 있다**는 점이다.<br>
예제로 살펴보자. 아래처럼 객체를 가리키는 변수(`person`)를 다른 변수(`copy`)에 할당하면 **원본의 참조 값을 복사해 전달**한다.<br>
우리는 이를 **Pass by reference, 참조에 의한 전달**이라고 부른다.

이때, 각 원본 `person` 변수와 사본 `copy` 변수는 저장된 메모리 주소는 다르지만,<br>
동일한 참조 값을 갖는다. 즉, 동일한 객체를 가리킨다.<br>
이것은 두 개의 식별자가 하나의 객체를 공유한다는 것을 의미하는데,<br>
`person`과 `copy` 중 **어느 한쪽에서 객체를 변경하면 서로 영향을 주고받는다.**

```javascript
var person = {
  name: 'Lee'
};

// 참조값을 복사(얕은 복사)
var copy = person;
```
<img src="https://github.com/user-attachments/assets/b57b71c1-45b6-4c14-8185-018c40bc746b" width="700px" />

끝.
