# javascript아리송한 상황들

Javascript는 비교적 쉽다고 느껴진다. 우리가 대충 코드를 작성해도 똑똑한 브라우저가 알아서 실행해주기 때문이다. 하지만 그만큼 남용될 수 있는 여지도 많다.

**잘못 된 코드를 작성하기 쉽다.**

```javascript
console.log=14
console.log() //Uncaught TypeError...
```

자바처럼 빌드과정이 필요한 언어에서는 발생하지 않는 문제이다. 빌드 과정에서 문법 검사를 실행하기 떄문에 프로그램 실행 전에 이런 에러를 방지할 수 있다.

하지만 JS처럼 빌드 과정이 없는 언어는.. 코드를 작성하고 브라우저에서 실행을 해봐야 에러를 확인할 수 있다.

    '1' + 1 = '11' //더하기 연산을 하면 숫자가 문자열이 된다.
    '2' * 3 = 6 //곱하기 연산을 하면 문자열이 숫자가 된다.
    1 + '2' + 3 * 4 = 1212 

이렇게 JS는 문법 체크과정이 없기 때문에 모든 브라우저에서 직접 코드를 실행해봐야 동작을 보장할 수 있다. 이렇게 코드를 일일이 확인하는 방법 중 하나가  테스트이다. 코드를 직접 실행해서 결과를 검증한다.

# 몇 가지 테스트 개념들

### 단위(Unit) 테스트

특정 조건에서 어떻게 작동해야 할지 정의 한것이다. 다시 말해, input에 따라 계산한 결과를 output으로 내놓는 것이다. 이것을 단위라고 하는데.. 떠오르는 것? 바로 함수이다. 단위 테스트에서 말하는 단위는 함수를 뜻한다. 즉, 함수를 테스트 하는 것을 단위 테스트라고 한다.

단위 테스트는 세가지로 나눌 수 있다. 준비, 실행, 단언. 

input을 **준비**하는 단계

input을 전달해서 함수를 **실행**하는 단계

결과를 검증하는, **단언**하는 단계

## TDD

어떤 함수의 기능을 만들어야 된다고 생각해보자. 그럼 바로 기능을 코딩하려고 할테지만 TDD에서는 먼저 기능을 테스트 할 수 있는 테스트 코드를 만든다. 이것을 단위 테스트라고 한다. 단위 테스트가 TDD의 첫 번째 단계에 해당한다. 이 상태에서 테스트 코드를 실행하면... 당연히 테스트에 실패한다. 왜냐면 아직 기능코드가 없기 때문이다. 이렇게 실패하는 테스트 코드를 만드는 단계를 **실패(적색) 단계**라고 한다.

그리고 나서 함수의 기능코드를 작성한다. 잘 작성할 필요가 없고 테스트에 통과할 정도로만 구현하면 된다. 그리고 테스트를 실행해서 성공하면 이 단계를 **성공(녹색) 단계** 라고 한다.

TDD를  하는 목적은 품질 높은 코드를 만들기 위해서다. 아직은 그 정도의 품질은 아니다. 리팩토링이 필요하다. 추상화 하고 확장가능 하고 또 중복 되지 않은, 소프트 웨어 공학 원칙에 맞는 그런 코드를 만들어야 한다. 

개발자는 이 부분에서 상당히 조심스러워 하게 된다. 왜냐면 기존 로직을 유지하면서 코드를 리팩토링 해야하기 때문이다. 내가 개선한 코드가 기존 로직대로 돌아간다는 것을 보장할 수 있어야 하기 때문에 굉장히 부담스럽다.

TDD를 하게 되면 테스트 코드의 성공 여부에 따라서 내가 작성한 리팩토링이 확실하다라는 것을 보장받을 수 있다. 이 단계를 TDD에서는 리팩터 단계라고 한다.

TDD에서는 Red - Green - Blue의 주기를 반복하며 코드의 품질을 높이는 것이 핵심이다. 이렇게 되면 자연스럽게 테스트하기 쉬운 코드를 만들어내개 된다. 쉽게 테스트 하기 위해서는 함수가 하나의 기능만 하게  설계하게 될 것이다. 

# 설치

대부분의 프로그래밍 언어는 테스트를 할 수 있는 프레임웍이 있다. 여러가지가 있지만... 뭐든 하나만 제대로 사용하고 나면 나머지 프레임을 사용하는 것은 어렵지 않다. 무엇를 사용하든 테스트 개념만 익히면 다른 도구는 금방 익숙해진다.

Jamine의 두가지 설치 방법

- 스탠드얼론(standalone)
- 카르마(karma)와 함께 설치하는 방법 (자동화)

모든 Jamine코드를 브라우저에 올려서 실행하는 방법이다. 간단하게 실행 결과를 볼 수 있지만 실무에서 많이 쓰이진 않는다. 

수많은 테스트 코드를 실행하고 결과를 확인하려면 자동화 할 수 있는 방법이 필요한데 karma 라이브러리와 연동해서 사용하는 것이 실무에서 많이 사용하는 방법이다.

https://github.com/jasmine/jasmine/releases

이 강의에서는 Jasmine 2.7.0을 다운 받아서 진행한다. 다운로드 후 압축을 풀고 SpecRunner.html 파일을 확인해보자.

**테스트 러너(Test Runner)란**

테스트 코드를 실행하는 것을 테스트 러너라고 한다. 여기서는 standalone으로 자스민을 설치했기 때문에 SpecRunner.html 파일이 테스트 러너 역할을 한다. 반면 Karma로 자동화하게 되면 브라우저가 아니라 karma가 테스트 러너가 된다. 명령으로 실행해서 자동으로 테스트를 돌릴 수 있다.

```javascript
<script>
    describe('hello world', ()=> { // 테스트 스윗: 테스트 유닛들의 모음 
      it('true is true', ()=> { // 테스트 유닛: 테스트 단위
        expect(true).toBe(true) // 매쳐: 검증자 
      })
    })
</script>
```

describe 함수는 테스트 꾸러미다. 테스트 케이스의 모음이라고 본다. 함수 하나하나를 테스트 할 때  describe를 사용한다.  참고로 describe는 중첩해서 사용할 수 있다. 무한대로 늘어날 수 있다.

it 함수는 테스트 케이스를 만들 때 사용한다. 함수의 기능을 테스트 할 때 사용한다. 첫번째 인자로 함수 기능의 스펙을 문자열로 넘겨주고, 테스트 코드를 구현할 수 있는 함수를 넘겨준다.

- 테스트 꾸러미 (Test Suite) 
- describe ('테스트 설명', 테스트 구현 함수)
- 테스트 스펙(Test Spec)
- it ('테스트 설명', 기대식을 가진 테스트 구현 함수)
- 기대식과 매쳐
- expect(결과 값).toBe(기대하는 값)
- 스파이
- spyOn(감시할 객체, 감시할 메서드)

# 테스트 할 수 없는 코드

프론트엔드 코드 테스트는 비교적 어렵다. 테스트 할 수 없게 작성했기 때문이다.

우리가 작성하는 코드는 의외로 테스트 하기 힘든 경우가 많다. 

```javascript
<button onlcick="counter++; countDisplay()">증가</button>
<span id="counter-display">0</span>

<script>
var counter = 0;

function countDisplay(){
	var el = document.getElementById('counter-display');
	el.innerHTML = counter;
}
</script>
```

이 코드의 문제를 살펴보자.

버튼 코드를 한번 보자.

```javascript
<button onlcick="counter++; countDisplay()">증가</button>
```

 한 줄이지만 여러가지 역할을 수행하고 있다. 증가 버튼을 만들고 있고, 클릭 이벤트에 동작하는 코드를 가지고 있다. 여러가지 역할을 하고 있다.

소프트웨어 공학 원칙에는 단일 책임의 원칙이 있다. OOP에 해당하는 이야기지만 여기서도 해당하는 원칙이다. 하나의 코드는 한가지 역할만 해야한다. 그래야 읽기 쉽고 명확한 코드가 된다.

다음 코드를 보자.

```javascript
var counter = 0;
```

counter 변수가 전역인 것이 문제이다. 전역 변수 사용은 전역적인 안티 패턴이다. 다른 모듈, 또는 코드와 충돌할 수 있는 여지가 너무 많다. 쓰지 말거나 쓰더라도 제한해서 써야 한다.

다음 코드도 보자.

```javascript
var el = document.getElementById('counter-display');
```

이건 id가 'counter-display'일 경우에만 사용할 수 있는 함수이다. 아이디가 달라지거나 다른 셀렉터를 사용한다면 전혀 사용할 수 없는 코드이다. 그럴 경우 함수 코드 자체를 수정해야 한다.

소프트웨어 공학중에는 오픈 클로즈 원칙도 있다. 짧게 이야기하면 확장에는 열려있고 변경에는 닫혀있어야 한다는 것이다.  함수로 예를 들면 함수가 쉽게 변경되면 안되다는 것이다. 대신 기능을 추가할 것이라면 새로운 함수를 만드는것이 아니라 기존 함수를 확장해서 만들어야 한다는 것이다.

1. 관심사의 분리
2. 전역 변수의 충돌
3. 재사용이 어려움

# 모듈 패턴

하나의 문제에는 여러가지 해결 방법이 있을 수 있다. 그 중에 최적의 방법을 찾아서 해결하는 것이 좋은 개발자의 덕목이다. 어떤 문제는 개발자들이 자주 쓰는 해결 방법이 있는데 이것을 우리는 패턴이라고 한다.

JS의 경우는 문제 해결에 가장 많이 해결되는 패턴이 모듈 패턴이다.

모듈 패턴에는 두가지가 있다.

1. 임의 모듈 패턴
2. 즉시 실행 함수 모듈 패턴

### 임의 모듈 패턴

```javascript
var App = App || {}

/*
App이라는 네임스페이스를 정의하고 Person이라는 모듈을 정의했다.
사실은 getName과 setName이라는 메서드로 이루어진 객체를 반환하는 함수이다.
그리고 모듈을 생성할 때 God이라는 또 다른 객체를 주입받고 있다.
God모듈이 Name을 만들어 내는 역할을 하기 때문이다.
*/
App.Person = function (God) {
	/*
	name변수는 모듈 안에서만 접근 가능하도록 이루어지고 있다.
  리턴되는 객체의 메서드 getName, setName에서 사용하기 떄문에 close변수로 저장되는 것이다.
  */
	var name = God.makeName();
	
  //Api를 노출한다.
	return {
		getName: function() { return name },
		setName : function(newName) { name = newName}
	}
}
```

이렇게 사용한다.

```javascript
const Person = App.Person(god)
person.getName()
```

### 즉시 실행 함수(IIFE) 모듈 패턴 (싱글톤 인스턴스가 됨)

```javascript
var App = App || {}
App.Person = (function () {
	let name = ""

	return {
		getName (God) {
			name = name || God.makeName()
			return name
		},
		setName (newName) { name = newName }
	}
}}() //함수 즉시 실행한다. 싱글톤이다.
```

임의 모듈 패턴과 굉장히 유사하다. 단, 다른점은 모듈을 정의한 직후에 객체를 바로 생성해 버린다는 것이다. 싱글톤일 경우에 사용한다.

객체가 여러개 필요할 경우엔 임의 객체 모듈을 쓰지만 단 하나의 객체만 필요하다만 위와 같은 싱글톤 패턴을 사용하게 된다.

```javascript
App.Person.getName(God)
```

이런 모듈 패턴을 사용하려면 두가지 잘 지켜야 한다.

1. 단일 책임 원칙에 따라 모듈은 한가지 역할만 해야 한다. 그 역할만 집중함으로써 모듈을 더욱 튼튼하게 만든다. 기능이 한 개이기 때문에 테스트하기도 더 쉽다.
2. 모듈 자신이 사용할 객체가 있다면 의존성 주입형태로 제공해야 한다. 모듈 하나로만 로직을 다 작성할 순 없다. 역할에 따라 여러개의 모듈을 만들어야 하는데 이 때 모듈간의 의존관계가 생긴다. 

# 클릭 카운터 모듈

보이는 부분과 보이지 않는 부분을 나눠서 생각해보자.

화면에 보이는 부분- 버튼과 카운트 값을 출력하는 부분

화면에 보이지 않는 부분 - 클릭 이벤트을 받고 카운트 값을 갱신하는 부분

일단 화면에 보이지 않는 부분을 만들어보자. ClickCounter모듈는 카운터 데이터를 다루는 모듈이다. 전역 공간에 있던 counter변수를 clickCounter안에 가지고 와서 관리하면 더 편리하다.

### 첫번째 스펙

clickCounter모듈의 getValue() 카운터 값을 반환한다.

[ClickCounter.spec.js] 실패(적색) 단계 

첫번째 스펙으로 테스트 코드를 작성해 보자.

```javascript
describe('App.ClickCounter', ()=> {
  describe('getValue()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      const counter = App.ClickCounter();
      expect(counter.getValue()).toBe(0);
    })
  })
})
```

[ClickCounter.js] 성공(녹색) 단계

이제 테스트를 통과하도록 모듈 코드를 만들어 본다. 

```javascript
App.ClickCounter = () => {
    return {
        getValue() {
            return 0
        }
    }
}
```

[ClickCounter.js] - 리팩터 단계

카운터 값은 버튼을 클릭할 때마다 변경될 것 이기 때문에 상수 값을 사용하지 않고 변수로 빼는 것이 좋다.

```javascript
App.ClickCounter = () => {
		let value = 0
    return {
        getValue() {
            return value
        }
    }
}
```

이렇게 안심하고 리팩토링 할 수 있는 이유는 이미 작성한 테스트 코드 때문이다. 리팩토링 한 다음에 올바르게 동작 한다는 것을 보장하기 위해서는 테스트들 돌려보면 바로 알 수 있다.

### 두번째 스펙

ClickCounter 모듈의 increase()는 카운터 값을 1만큼 증가한다.

[ClickCounter.spec.js] 실패(적색) 단계 

```javascript
describe('App.ClickCounter', ()=> {
  describe('getValue()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      const counter = App.ClickCounter()
      expect(counter.getValue()).toBe(0)
    })
  })

  describe('increase()', ()=> {
    it('카운터를 1 올린다', ()=> {
      //준비
      const counter = App.clickCounter();

      //실행
      counter.increase()

      //단언
      expect(counter.getValue()).toBe(1)
    })
  })
})
```

[ClickCounter.js] - 성공(녹색) 단계

```javascript
var App = App || {}

App.ClickCounter = () => {
  let value = 0

  return {
    getValue() {
      return value
    },
    increase() {
        value++;
    }
  }
}
```

[ClickCounter.spec.js] - 리팩터 단계

중복된 테스트 코드가 보인다. 자스민의 beforeEach 함수는 it 함수 호출 직전에 실행된다.

```javascript
describe(()=> {
	beforeEach(()=> { //실행순서 1
	afterEach(()=>{   //실행순서 3
	it(()=>{          //실행순서 2       
```

물론 beforeEach, afterEach를 사용하지 않으면 it 함수만 실행된다.

```javascript
describe('App.ClickCounter', ()=> {
	let counter
	beforeEach(()=>{
		counter = App.ClickCounter()
	})

  describe('getValue()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      expect(counter.getValue()).toBe(0)
    })
  })

  describe('increase()', ()=> {
    it('카운터를 1 올린다', ()=> {
			//실행
      counter.increase()
      //단언
      expect(counter.getValue()).toBe(1)
    })
  })
})
```

그러나 아직도 구린내가 난다. 초기 값이 0이 아닐 경우 increase() 실행 결과가 1이 아닐 수 있다.

```javascript
describe('App.ClickCounter', ()=> {
	let counter
	beforeEach(()=>{
		counter = App.ClickCounter()
	})

  describe('getValue()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      expect(counter.getValue()).toBe(0)
    })
  })

  describe('increase()', ()=> {
    it('카운터를 1 올린다', ()=> {
		  const initialValue = counter.getValue();
			//실행
      counter.increase()
      //단언
      expect(counter.getValue()).toBe(initialValue + 1)
    })
  })
})
```

# 클릭 카운트 뷰 모듈

이번에는 화면에 보이는 부분을 만들어보자.

클릭 카운터 모듈에서 만든 카운터 데이터를 DOM에 반영에서 화면에 보이게 하는 것이 이 모듈의 역할이다. 데이터를 출력할 뿐 아니라 클릭 이벤트 핸들러는 바인딩 하는 것도 담당한다.

### 첫번째 스펙

ClickCountView 모듈의 updateView() 는 카운트 값을 출력한다.

그런데.. 데이터를 조회할 ClickCounter를 어떻게 얻을 수 있을까? 게다가 데이터를 출력할 돔 엘리먼트는 어떻게 테스트 할까?

주입하자!

- ClickCounter 객체를 만들어서 ClickCounteView 모듈의 생성인자로 전달한다.
- 데이터를 출력할 돔 엘리먼트도 같은 방법으로 인자로 전달한다.

TDD로 개발을 하게 되면 이런 의존 관계이 있는 모듈을 주입하는 경향이 생긴다. 주입된 객체들은 하나의 역할만 수행하기 때문에 단일 책임의 원칙도 지킬 수 있다.

[ClickCountView.spec.js] 실패 단계

```javascript
describe('App.ClickCountView', ()=> {
  describe('updateView()', ()=> {

    let clickCounter, updateElement, view

    beforeEach(()=>{
      clickCounter = App.ClickCounter()
      updateElement = document.createElement('span')
      view = App.ClickCountView(clickCounter, updateElement)
    })

    it('ClickCounter의 getValue() 값을 출력한다', ()=> {
      const counterValue = clickCounter.getValue()
      view.updateView()
      expect(updateElement.innerHTML).toBe(counterValue.toString())
    })
  })
})
```

[ClickCountView.js] 성공 단계

```javascript
var App = App || {}

App.ClickCountView = ((clickCounter, updateElement) => {
    return {
        updateView(){
            updateElement.innerHTML = clickCounter.getValue()
        }
    }
})
```

ClickCountView에 의존성 주입이 되어있는지는 어떻게 체크할까? 모듈을 사용하는 측에서 의존 모듈을 넘겨주지 않으면 ClickCountView 모듈은 제대로 동작하지 않을 수 있다.

자스민 프레임웍에는 toThrowError라는 매처 함수가 있다. expect 안에 예외를 던지는 함수를 전달을 하면 toThrowError 함수로 에러 여부를 확인할 수 있다.

[ClickCountView.spec.js] 실패 단계

```javascript
describe('App.ClickCountView', ()=> {
  let clickCounter, updateElement, view
  beforeEach(()=>{
    clickCounter = App.ClickCounter()
    updateElement = document.createElement('span')
    view = App.ClickCountView(clickCounter, updateElement)
  })
  
  it('clickCounter를 주입하지 않으면 에러를 던진다', ()=>{
    const clickCounter = null
    const updateElement = document.createElement('span')
    const actual = () => App.ClickCountView(clickCounter, updateElement)
    expect(actual).toThrowError()
  })
  
  it('updateElement를 주입하지 않으면 에러를 던진다', ()=>{
    const clickCounter = App.ClickCounter()
    const updateElement = null
    const actual = () => App.ClickCountView(clickCounter, updateElement)
    expect(actual).toThrowError()
  })
  
  describe('updateView()', ()=> {
    it('ClickCounter의 getValue() 값을 출력한다', ()=> {
      const counterValue = clickCounter.getValue()
      view.updateView()
      expect(updateElement.innerHTML).toBe(counterValue.toString())
    })
  })
})
```

[ClickCountView.js] 성공 단계

```javascript
var App = App || {}

App.ClickCountView = ((clickCounter, updateElement) => {
    if (!clickCounter) throw new Error(App.ClickCountView.messages.noClickCounter)
    if (!updateElement) throw new Error(App.ClickCountView.messages.updateElement)

    return {
        updateView(){
            updateElement.innerHTML = clickCounter.getValue()
        }
    }
})

App.ClickCountView.messages = {
    noClickCounter: 'clickCount를 주입해야 합니다',
    noUpdateEl: 'updateEl를 주입해야 합니다'
  }
```

### 두번째 스펙

ClickCountView 모듈의 icreaseAndUpdateView()는 카운트 값을 증가 시키고 그 값을 출력한다.

카운트 값을 증가 시킨다는 건 결국 ClickCounter 모듈의 increase함수가 실행되었다는 뜻이고,

그 값을 출력한다는 것은 ClickCountView 모듈의 updateView 함수가 실행되었다는 뜻이다.

**테스트 더블**

TDD 용어중에는 테스트 더블이라는 개념이 있다. 

단위테스트의 패턴으로 테스트 하기 곤란한 컴포넌트를 대체 하는 것을 말한다. 함수를 테스트 하기 위해서 어떤 행동만 흉내내는 것이다.

아래 5가지를 통칭해 테스트 더블이라고 한다.

- 더미 - 함수 인자를 채우기 위해 사용한다.
- 스텁 - 더미를 발전한 것으로 리터값이 있는 것을 말한다.
- 스파이 - 스텁과 유사하지만 내부적으로 계속 기록을 남긴다.
- 페이크 - 스텁이 리턴 값이 하드코딩 되있다면 페이크는 실제 값을 리턴한다.
- 목 - 더미, 스텁, 스파이를 혼합한 형태를 말한다.

자스민에서는 테스트 더블을 스파이스(spices)라고 부른다.

spyOn() 함수를 사용해서 스파이스를 만들어보자.

```javascript
/*
clickCounter 모듈의 increase 함수를 감시하도록 설정한다.
첫번째는 감지할 객체, 두번째는 감지할 함수다.
*/
spyOn(MyApp, 'foo')

//특정행동을 한 뒤
bar()

//감시한 함수가 실행되었는지 체크한다.
expect(MyApp.foo).toHaveBeenCalled()
```

즉 bar() 함수가 MyApp.foo() 함수를 실행하는지 검증하는 코드이다.

[ClickCountView.spec.js]

```javascript
describe('App.ClickCountView 모듈', () => {
  let udpateEl, clickCounter, view

  beforeEach(()=> {
    updateEl = document.createElement('span')
    clickCounter = App.ClickCounter(); 
    view = App.ClickCountView(clickCounter, updateEl)
  })
  
  describe('네거티브 테스트', ()=> {
    it('ClickCounter를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(null, updateEl)
      expect(actual).toThrowError(App.ClickCountView.messages.noClickCounter)
    })

    it('updateEl를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(clickCounter, null)
      expect(actual).toThrowError(App.ClickCountView.messages.noUpdateEl)
    })
  })

  describe('updateView()', () => {
    it('ClickCounter의 getValue() 실행결과를 출력한다', ()=> {
      const counterValue = clickCounter.getValue()
      view.updateView()
      expect(updateEl.innerHTML).toBe(counterValue.toString())
    })
  })

  describe('increaseAndUpdateView()는', ()=> {
    it('ClickCounter의 increase 를 실행한다', ()=> {
      spyOn(clickCounter, 'increase')
      view.increaseAndUpdateView()
      expect(clickCounter.increase).toHaveBeenCalled()
    })
    
    it('updateView를 실행한다', ()=> {
      spyOn(view, 'updateView')
      view.increaseAndUpdateView()
      expect(view.updateView).toHaveBeenCalled()
    })
  })
})
```

[ClickCountView.js]

```javascript
var App = App || {}

App.ClickCountView = (clickCounter, updateEl) => {
  if (!clickCounter) throw new Error(App.ClickCountView.messages.noClickCounter)
  if (!updateEl) throw new Error(App.ClickCountView.messages.noUpdateEl)
  
  return {
    updateView() {
      updateEl.innerHTML = clickCounter.getValue()
    },

    increaseAndUpdateView() {
      clickCounter.increase()
      this.updateView()
    }
  }
}

App.ClickCountView.messages = {
  noClickCounter: 'clickCount를 주입해야 합니다',
  noUpdateEl: 'updateEl를 주입해야 합니다'
}
```

### 세번째 스펙

클릭 이벤트가 발생하면 increaseAndUpdateView를 실행한다.

여기서도 주입 패턴을 사용한다. 카운터 값을 출력할 돔 엘리먼트(updateElement)를 주입했듯이 클릭 이벤트 핸들러(increaseAndUpdateView)를 바인딩할 돔 엘리먼트(triggerElement)를 주입하자.

[ClickCountView.spec.js] 실패 단계

```javascript
describe('App.ClickCountView 모듈', () => {
  let updateElement, clickCounter, view, triggerElement

  beforeEach(()=> {
    updateElement = document.createElement('span')
    triggerElement = document.createElement('button')
    clickCounter = App.ClickCounter(); 
    view = App.ClickCountView(clickCounter, {updateElement, triggerElement})
  })
  
  describe('네거티브 테스트', ()=> {
    it('ClickCounter를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(null, {updateElement})
      expect(actual).toThrowError(App.ClickCountView.messages.noClickCounter)
    })

    it('updateEl를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(clickCounter, {triggerElement})
      expect(actual).toThrowError(App.ClickCountView.messages.updateElement)
    })
  })

  describe('updateView()', () => {
    it('ClickCounter의 getValue() 실행결과를 출력한다', ()=> {
      const counterValue = clickCounter.getValue()
      view.updateView()
      expect(updateElement.innerHTML).toBe(counterValue.toString())
    })
  })

  describe('increaseAndUpdateView()는', ()=> {
    it('ClickCounter의 increase 를 실행한다', ()=> {
      spyOn(clickCounter, 'increase')
      view.increaseAndUpdateView()
      expect(clickCounter.increase).toHaveBeenCalled()
    })
    
    it('updateView를 실행한다', ()=> {
      spyOn(view, 'updateView')
      view.increaseAndUpdateView()
      expect(view.updateView).toHaveBeenCalled()
    })
  })

  it('클릭 이벤트가 발생하면 increseAndUpdateView를 실행한다', ()=> {
    spyOn(view, 'increaseAndUpdateView')
    //click
    triggerElement.click()
    expect(view.increaseAndUpdateView).toHaveBeenCalled()
  })
})
```

[ClickCountView.js] 성공 단계

```javascript
var App = App || {}

App.ClickCountView = (clickCounter, options) => {
  if (!clickCounter) throw new Error(App.ClickCountView.messages.noClickCounter)
  if (!options.updateElement) throw new Error(App.ClickCountView.messages.updateElement)
  
  const view = {
    updateView() {
        options.updateElement.innerHTML = clickCounter.getValue()
    },

    increaseAndUpdateView() {
      clickCounter.increase()
      this.updateView()
    }
  }
  options.triggerElement.addEventListener('click', ()=>{
    view.increaseAndUpdateView()
  })
  
  return view
}

App.ClickCountView.messages = {
  noClickCounter: 'clickCount를 주입해야 합니다',
  updateElement: 'updateElement를 주입해야 합니다'
}
```

테스트 코드로 요구사항을 만들고 이것을 기반으로 모듈 로직을 만들어내는 과정을 경험했다. 처음에는 매우 느릴  수 밖에 없다. 이런 과정이 견고한 로직을 만들어내게 되고 오히려 개발 속도가 빨라지게 된다. 익숙해지냐의 문제이다. 스스로 이런 과정을 반복하면서 익숙해지는 수 밖에 없다.

# 뭐가 개선 된거지~?

### 모듈을 이용해서 화면 만들기

```javascript
<html>
  <body>
    <span id="counter-display"></span>
    <button id="btn-increase">Increase</button>

    <script src="ClickCounter.js"></script>
    <script src="ClickCountView.js"></script>

    <script>
      (() => {
        const clickCounter  = App.ClickCounter()
        const updateEl = document.querySelector('#counter-display');
        const triggerEl = document.querySelector('#btn-increase')
        const view = App.ClickCountView(clickCounter, {updateEl, triggerEl}) 
        view.updateView();
      })()
    </script>
  </body>
</html>
```

### 

# 추가 요구 사항도 쉽게 받을 수 있는 코드 만들기

### 클릭카운터 모듈 - 세번째 스펙

ClickCounter 모듈은 데이터를 주입 받는다.

요구사항은 매번 변하기 마련이다. 증가 버튼 뿐 아니라 감소 버튼도 필요 하거나 증가 버튼을 클릭하면 2씩 증가해야 될지도 모른다.

증가 버튼 뿐 아니라 감소 버튼을 추가하려면 어떻게 해야할까 ? 버튼도 두 개 있어야 하고 이에 상응 하는 클릭 카운터도 두 개 있어야 한다. 증가는 increase, 감소는 decrease 함수를 사용해야 한다. 

그런데 클릭 카운터는 데이터를 클로즈 변수로 간직하고 있기 때문에 모듈 간에 공유를 할 수 가 없다.증가를 위한 클릭 카운터와 감소를 위한 클릭 카운터를 따로 만들게 되면 각자 독립적인 카운터 밸류를 가지는 상황이 발생하기 때문에.. 현재 하나의 카운터 값만 출력해서 이것을 증가 또는 감소해야 하는 상황과는 맞지 않는다.

그렇기 때문에 카운터 모듈은 데이터를 주입 받도록 하자. 공통의 데이터가 있고 클릭 카운터 객체가 그 공통의 객체를 주입 받게 만들자.

ClickCounter.spec.js

~~~javascript
describe('App.ClickCounter', ()=> {
  let counter, data

  it('초기값을 주입하지 않으면 에러를 던진다', () => {
    const actual = () => (counter = App.ClickCounter())
    expect(actual).toThrowError();
  })

  beforeEach(()=> {
    data = {value : 0}
    counter = App.ClickCounter(data)
  })

  describe('getValue()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      expect(counter.getValue()).toBe(0)
    })
  })

  describe('increase()', ()=> {
    it('카운터를 1 올린다', ()=> {
      const initialValue = counter.getValue()
      counter.increase()
      expect(counter.getValue()).toBe(initialValue + 1)
    })
  })
})
~~~



ClickCounter.js

~~~javascript
var App = App || {}

App.ClickCounter = _data => {
  if(!_data) throw Error('_data')

  const data = _data;

  /*
  데이터를 공유하기 위해서는 기존처럼 정수 같은 원시형을 사용 할 수 없다.
  생성 인자로 넘겨주면 그 값은 복사되기 떄문이다.
  그러므로 레퍼런스로 받는 객체를 사용해야 한다.
  */

  data.value = data.value || 0

  return {
    getValue() {
      return data.value
    },

    increase() {
      data.value++
    }
  }
}
~~~



ClickCounterView.spec.js

~~~javascript
describe('App.ClickCountView 모듈', () => {
  let udpateEl, clickCounter, view

  beforeEach(()=> {
    const data = { value: 0 }
    clickCounter = App.ClickCounter(data)
    updateEl = document.createElement('span')
    triggerEl = document.createElement('button') 
    view = App.ClickCountView(clickCounter, {updateEl, triggerEl})
  })
  
  describe('네거티브 테스트', ()=> {
    it('ClickCounter를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(null, updateEl)
      expect(actual).toThrowError(App.ClickCountView.messages.noClickCounter)
    })

    it('updateEl를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(clickCounter, {triggerEl})
      expect(actual).toThrowError(App.ClickCountView.messages.noUpdateEl)
    })

    it('triggerEl를 주입하지 않으면 에러를 던진다', ()=> {
      const actual = () => App.ClickCountView(clickCounter, {updateEl})
      expect(actual).toThrowError(App.ClickCountView.messages.noTriggerEl)
    })
  })

  describe('updateView()', () => {
    it('ClickCounter의 getValue() 실행결과를 출력한다', ()=> {
      const counterValue = clickCounter.getValue()
      view.updateView()
      expect(updateEl.innerHTML).toBe(counterValue.toString())
    })
  })

  describe('increaseAndUpdateView()는', ()=> {
    it('ClickCounter의 increase 를 실행한다', ()=> {
      spyOn(clickCounter, 'increase')
      view.increaseAndUpdateView()
      expect(clickCounter.increase).toHaveBeenCalled()
    })
    
    it('updateView를 실행한다', ()=> {
      spyOn(view, 'updateView')
      view.increaseAndUpdateView()
      expect(view.updateView).toHaveBeenCalled()
    })
  })

  it('클릭 이벤트가 발생하면 increaseAndUpdateView를 실행한다', ()=> {
    spyOn(view, 'increaseAndUpdateView')
    triggerEl.click()
    expect(view.increaseAndUpdateView).toHaveBeenCalled()
  })
})
~~~



### 클릭카운터모듈 - 네번째 스펙

ClickCounter 모듈의 increase 함수는 대체 될 수 있다.

increase 함수는 올릴 수도 있지만 내릴 수도 있어야 한다. 이름 변경 : increase -> count

ClickCounter.spec.js

~~~javascript
describe('App.ClickCounter', ()=> {
  let counter
  const data = { value: 0 }

  it('초기값을 주입하지 않으면 에러를 던진다', () => {
    const actual = () => (counter = App.ClickCounter())
    expect(actual).toThrowError()
  })

  beforeEach(()=> (counter = App.ClickCounter(data)))

  describe('getValue()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      expect(counter.getValue()).toBe(0)
    })
  })

  describe('count()', ()=> {
    it('카운터를 1 올린다', ()=> {
      const initialValue = counter.getValue()
      counter.count()
      expect(counter.getValue()).toBe(initialValue + 1)
    })
  })

  describe('setCountFn()', () => {
    it('인자로 함수를 넘기면 count()를 대체한다', () => {
      const add2 = value => value + 2
      const expected = add2(data.value)
      /*
      setCountFc() 직후에 count() 함수를 체이닝 해준 것을 눈여겨 볼 필요가 있다.
      실제 구현 할 때도 이렇게 체이닝 할 수 있게 해주면 코드를 사용 할 수 있는 측면에서도 간단하게 코딩 할 수 있다.
      */
      counter.setCountFn(add2).count()

      const actual = counter.getValue()
      expected(actual).tobe(expected);
    })
  })
})
~~~



ClickCounter.js

~~~javascript
var App = App || {}

App.ClickCounter = _data => {
  if (!_data) throw Error('_data')
  const data = _data
  data.value = data.value || 0

  return {
    getValue() {
      return data.value
    },

    count() {
      data.value++
    },

    setCountFn(fn) {
      this.count = () => (data.value = fn(data.value))
        return this
    }
  }
}

~~~



# 정리

### 화면에 붙여 보자

index.html

~~~javascript
<html>
  <body>
    <button id="btn-desc">-</button>
    <span id="counter-display"></span>
    <button id="btn-inc">+</button>

    <script src="ClickCounter.js"></script>
    <script src="ClickCountView.js"></script>

    <script>
      (() => {
        const data = { value : 0 }
        const counterDesc = App.ClickCounter(data).setCountFn(v => v - 1)
        const counterInc = App.ClickCounter(data).setCountFn(v => v + 1)

        const updateEl = document.querySelector('#counter-display')
        const btnDesc = document.querySelector('#btn-desc')
        const btnInc = document.querySelector('#btn-inc')

        const decsCounterView = App.ClickCountView(counterDesc, {updateEl, triggerEl : btnDesc})
        const incCounterView = App.ClickCountView(counterInc, {updateEl, triggerEl : btnInc})

        decsCounterView.updateView()
      })()
    </script>
  </body>
</html>
~~~



### 정리 - 스펙 4

자바스크립트 문법 특성에 대해서 살펴 봤었다. 타입 체크처럼 다른 프로그래밍 언어가 가지고 있는 단계가 없기 때문에 쉽게 에러가 발생할 수 있다는 점이 자바스크립트의 문제점 이였다. 따라서 자바스크립트 코드를실 제로 실행해서 그 결과를 확인하는 방법이 필요한데 그것이 테스트였고 즉 TDD로 이용하기로 결정했다. 실습에 앞서 테스트 할 수 없는 기존 코드를 살펴 보았는데 여러가지 문제점이 있다는 것을 지적했었다. 문제가 많았지만 문제를 한마디로 해결하면 '테스트 할 수 없다' 였다. 모듈 패턴을 소개하면서 이 문제의 코드에 적용했고 TDD를 이용해서 단계적으로 리팩토링해 나갔다 .그래서 나온 결과가 클릭카운터 모듈과, 클릭카운터 뷰 모듈이였다. 이 두 개의 모듈은 쉽게 변경되지 않으면서도 다양한 요구사항을 처리할 수 있는 아주 유연한 코드가 되었다. 프론트개발에 있어서  TDD를 이용하는 방법을 살펴보았다. 이런 과정을 반복해서 몸에 익히는 것이 견고한 소프트웨어를 만들 수 있는 방법이다..


