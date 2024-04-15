typeScript는 우선 간단하게 타입을 자바스크립트에서 타입을 추가로 지정 해준다. 라고 생각하면 될것 같다

## 타입추론

타입스크립트는 타입 추론 기능을 활용이 가능하다.
`let a = 1` 이런 코드가 있다고 가정 했을 때 a의 타입을 보면 number로 자동으로 타입이 추론이 되어있는 것을 볼 수 있다.
타입 추론 기능을 통해 타입스크립트를 할 때 무분별하게 타입을 다 지정을 해주지 않아도 되는 것을 알게 되었다. 😭

## 기본적인 타입

String 타입
말 그대로 문자열 타입이고, 타입 지정은 `let a : string = '12'  ` 이렇게 지정하면 된다.
추가적으로 `let a : string[] = '12'  ` string 타입 뒤에 배열[]을 넣으면 []된 string 타입을 지정해주는 것과 같다 ex) ['hi', 'hello']

Number 타입
숫자 타입으로 숫자 타입만 사용하도록 타입을 지정한다.
타입지정은 ` let a : number = 1` 다음과 같이 작성 해준다.
String 타입과 마찬가지로 ` let a : number[] = 1` 을 사용하면 배열[]로 된 숫자형 타입만 지정한다 ex) [1,2,3]

Boolean 타입
True or False 타입만 인정하도록 타입 지정한다.  
 타입 지정은 `let a : boolean = true` 다음과 같이 작성 해준다
Boolean 타입 또한 `let a : boolean[] = true` 배열로 된 boolean 타입을 지정이 가능하다.

Object 타입
Object 타입 지정은 Object로 직접 명시하는 것이 아닌 프로퍼티의 타입을 일일이 지정해주면 된다.
`let player = {
		name = 'donny'
	}` 다음과 같이 코드가 있을 시 `player: {name: string}` 다음과 같이 작성해주면 된다 !!
만약 프로퍼티의 값이 있을 수도 있고, 없을 수도 있다. 이 때 optional parameter(?)을 사용하면 해결이 된다. ex) `player: {name?: string}`

## Type Alias(별칭)

타입스크립트를 하면서 타입이 반복될때가 종종 있는데 이를 Type Alias로 해결이 가능하다.
사용법은 `.type player = {name: string} ` 이렇게 따로 만들면 player라는 타입이 생겨서 이를 재활용해서 사용이 가능하다 !!
typeAlias 또한 무분별하게 막 사용하면 타입 지정에 혼란을 줄 수 있다. 그러므로 무분별하게 막 사용은 하지 않도록 하자

## return Type 지정

함수를 만들고 return 할 때 타입 지정하는 방법이 따로 있다. 어렵지는 않다
다음과 같은 함수가 있다고 생각해보자.
function age (anything: number) : string {
return name
}
함수 parameter 타입 지정은 위의 타입 지정하는것과 같이 하면 된다.
return 의 경우는 parameter 뒤의 return 하는 함수의 타입을 지정해주면 된다.  
 화살표 함수도 위와 같이 사용하면 된다. ex) const a = (age:number) : string => {}

## ReadOnly

readonly로 타입을 지정해주면 타입의 값을 변경하면 에러가 나도록 타입스크립트가 막아준다.
`type gender = string 
	type player = {
		name: string,
		age: number,
		readonly gender : 'male'
		}
	const donny : player= {
		name: 'donny',
		age: 12,
		gender : 'male'
	}`  
 이런 코드가 있을 때 gender에 male이 아닌 다른 값을 사용할 떄 타입 에러가 나는 것을 볼 수 있다.![[스크린샷 2024-04-12 17.10.44.png]]

## Tuple Type

튜플
간단하게 튜플에 대해 알아 보자면 길이와 타입이 고정되어있다고 보면 될 것같다.
ex) [number, string, boolean]

튜플 타입 사용이유?
튜플을 사용하는 이유는 간단하다.
array보다 조금더 엄격하게 관리하고 싶을 때 사용한다. 위의 예시처럼 api와 통신할 때 타입을 지정하면 배열의 타입이 더 관리가 잘 될 것이다.

## Unknown, Void

unKnown타입은 말 그대로 어떤 타입이될지 모를 때 사용한다.
unknwon 타입은 무작정 사용은 못하고 typeof로 어떤 타입인지 검사를 조건문을 걸어줘야 사용이 가능하다.
api 통신할 때 어떤 타입으로 오는지 모를 때 사용하는것 같다. 사용법은 `let a : unknown ` 다음과 같이 사용하고 a는 타입검사를 해서 사용이 가능하다.
`if(typeof a === number)` 이렇게 typeof를 사용해서 a가 number 타입일 때 반환할 수 있도록 만들면 된다.

Void 타입은 함수가 return을 하지 않을 때 사용하는 타입이다.

## Call Signiture

Call Signitures란 vsCode를 하다보면 함수 파라미터에 마우스를 올리면 알아서 타입이 보이는 데 이를 말한다.
왜 필요할까? react를 하다보면 props로 값을 전달해줄 때 값이 number인지 string인지 헷갈릴 수 있는데 이를 해결해줄 수 있다!!!
`function add (a:number, b: number) {
		return a + b
	}`
기본 함수 타입을 지정해줄 때는 다음과 같이 작성해야 한다. return 의 (number는 생략가능하다고 한다. number + number라서 그런거 같다).  
 나만의 add 함수의 call Signiture을 만들기 위해서는 화살표 함수로 만들면 된다.
`type Add = (a: number, b : number) => number`

## OverLoading

함수를 만들 때 Call Signitures를 활용해서 함수 매개변수와 리턴 타입을 지정해줄수 있었다.
Call Signiture 을 만들 때 하나의 문제점이 있는데 바로 OverLoading이라는 것이다. 말 그대로 덮어 씌우지는 거라고 생각하면 될것 같다. ![[code 9.png]]
위 코드를 보자 위 코드에서 Plus 타입을 정의하는데 b변수의 타입이 두가지가 올 때가 OverLoading 되었다고 볼 수 있다. 그래서 따로 b의 type을 지정해주고 사용해야 한다

![[code 10.png]]
또 다른 예시로는 매개변수의 타입이 다른게 아닌 c라는 매개변수가 있을수도 있고 없을수도 있는 상황일 때 OverLoading이 일어난다. OverLoading을 해결하기 위해 c에 옵셔널 체니잉(?)을 붙여주자!

## PolyMorphism과 Generic

PolyMorphism이란 다향성이라고 한다.
다향성이라는게 어떤 의미인지 코드로 한번 봐보자![[code 11.png]]
위 코드를 보면 SuperPrint는 함수로 void 아무것도 반환하지 않고 다양한 타입들을 반환한다. 근데 이렇게 하나하나 다 지정해주는 것은 좋지 않은데, 이를 해결해줄수 있는 것이 Generic Type이다.
Generic Type의 사용 방법은 ![[code 12.png]]
위 사진과 같고 타입을 지정할 때 <>대괄호로 아무 이름이나 작성하면 되는데 보통 T or V를 사용한다고 한다. Generic Type을 사용하면 위의 코드처럼 하나하나 타입을 지정해주지 않아도 타입이 알아서 지정이 된다. 이를 다향성이라고 하는것 같다.

## Generic Type

Generic Type은 정말 다양하게 사용이 가능하다 위의 처럼 사용하기도 하고 다른 방법으로도 사용이 가능하다.

```
type Add<E> = {
	name: string;
	extraInfo: E;
};
```

위 코드처럼 Add 타입에 제너릭을 사용해서 extraInfo에 Generic 사용이 가능하다
