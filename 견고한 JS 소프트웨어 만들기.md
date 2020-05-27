# javascriptjavascript견고한 JS 소프트웨어 만들기

강의 링크 [https://www.inflearn.com/course/tdd-견고한-소프트웨어-만들기/dashboard]([https://www.inflearn.com/course/tdd-%EA%B2%AC%EA%B3%A0%ED%95%9C-%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4-%EB%A7%8C%EB%93%A4%EA%B8%B0/dashboard](https://www.inflearn.com/course/tdd-견고한-소프트웨어-만들기/dashboard))



# 단위 테스트

여기서 말하는 단위는 언제나 그런 것은 아니지만 대게 함수를 말합니다.  즉, 함수를 테스트 하는 것을 단위 테스트라고 합니다.

단위테스트는 코드는 준비 - 실행 - 단언의 패턴을 따릅니다.

- 준비 (arrange) - input을 **준비**하는 단계
- 실행 (act) - input을 전달해서 함수를 **실행**하는 단계
- 단언 (assert) - 결과를 검증하는, **단언**하는 단계

# 테스트 러너

테스트 코드를 실행하는 것을 테스트 러너라고 합니다. 

```javascript
<script>
    describe('hello world', ()=> { // 테스트 스윗: 테스트 유닛들의 모음 
      it('true is true', ()=> { // 테스트 유닛: 테스트 단위
        expect(true).toBe(true) // 매쳐: 검증자 
      })
    })
</script>
```

describe 함수는 테스트 꾸러미 입니다. 테스트 케이스의 모음이라고 볼 수 있습니다. 함수 하나하나를 테스트 할 때 describe를 사용하고 무한대로 중첩해서 사용할 수 있습니다.

it 함수는 테스트 케이스를 만들 때 사용합니다.  함수의 기능을 테스트 할 때 사용 합니다. 첫번째 인자로 함수 기능의 스펙을 문자열로 넘겨주고, 테스트 코드를 구현할 수 있는 함수를 넘겨줍니다.

- 테스트 스윗 (Test Suit) - describe ('테스트 설명', 테스트 구현 함수)
- 테스트 스펙 (Test Spec) - it ('테스트 설명', 기대식을 가진 테스트 구현 함수)
- 기대식과 매쳐 - expect(결과값). toBe(기대하는 값)
- 스파이 - spyOn(감시할 객체, 감시할 메서드)

# 테스트 할 수 없는 코드

프론트엔드 코드는 비교적 테스트 하기 어렵습니다. 왜냐면?! 테스트 할 수 없게 작성했기 때문입니다.

아래 코드를 한번 살펴보도록 하죠.

```javascript
<button onClick="counter++; countDisplay()">증가</button>
<span id="counter-display">0</span>

<script>
var counter = 0;

function countDisplay(){
	var el = document.getElementById('counter-display');
	el.innerHTML = counter;
}
</script>
```

위 코드를 문제점을 바로 아실 수 있나요?

저는 ‘button의 onClick’ 이벤트에서 2개의 기능을 실행하고 있군' 정도만 보였는데요

위 코드엔 세개의 문제점이 있습니다.

### 관심사의 분리

```javascript
<button onlcick="counter++; countDisplay()">증가</button>
```

 한 줄 이지만 여러가지 역할을 수행하고 있습니다. 소프트웨어 공학의 단일 책임 원칙을 위반하고 있죠. OOP에 해당하는 이야기 이지만 여기에도 해당하는 원칙입니다. 하나의 코드는 한가지 역할만 해야합니다. 그래야 읽기 쉽고 명확한 코드가 됩니다.

### 전역 변수의 충돌

```javascript
var counter = 0;
```

전역 변수의 사용은 전역적인 안티 패턴입니다. 다른 모듈, 코드와 출동할 수 있는 여지가 많습니다. 전역 변수를 쓰지 말거나 쓰더라도 제약해서 사용해야 합니다.

### 재 사용의 어려움

```javascript
var el = document.getElementById('counter-display');
```

위 코드는 id가 counter-display일 경우에만 사용할 수 있는 코드입니다. 아이디가 달라 지거나 다른 셀렉터를 사용 한다면 전혀 사용할 수 없는 코드 입니다. 



# 테스트에 앞서 짚고 넘어갈 모듈 패턴

하나의 문제에는 여러가지 해결방법이 있을 수 있습니다. 그 중에 최적의 방법을 찾아서 해결하는 것이 좋은 개발자의 덕목입니다. (좋은 이야기네요) 어떤 문제는 개발자들이 자주 쓰는 해결 방법이 있는데 그것을 패턴이라고 부릅니다. 

JS의 경우 문제 해결에 가장 많이 해결되는 패턴이 모듈 패턴입니다.

모듈 패턴에 대해서는 일단 생략

모듈 패턴을 사용하려면 두 가지를 잘 지켜야 합니다.

1. 단일 책임 원칙에 따라 모듈은 한가지 역할만 해야 합니다. 한 가지 역할만 집중함으로써 모듈을 더욱 튼튼하게 만듭니다. 기능이 한 개이기 때문에 테스트도 용이합니다.
2. 
2. 모듈 자신이 사용할 객체가 있다면 의존성 주입 형태로 제공해야 합니다. 모듈 하나로만 모든 로직을 작성할 수 없기 때문에 역할에 따라 여러개의 모듈을 만들어야 하는데 이 때 모듈 간의 의존관계가 생깁니다.




# 기능별 모듈 나누기

위 문제의 코드의 기능을 크게 보이는 부분과 보이지 않는 부분을 나눠서 생각해 봅니다.

- 화면에 보이지 않는 부분 - 화면에 있는 버튼의 클릭 이벤트를 받아서 카운터 값을 갱신하는 기능
- 화면에 보이는 부분 - 버튼과 카운트 값을 출력하는 부분



# 본격 TDD로 개발해보기

일단 화면에 보이지 않는 부분부터 만들어 보겠습니다. 화면에 있는 버튼의 클릭 이벤트를 받아 카운터 값을 갱신하는 기능을  ClickCounter 모듈로 만들겠습니다.

## ClickCounter 모듈 (보이지 않는 부분) 

### Spec 1. getCount()는 카운터 값을 리턴한다.

TDD 1단계 - 실패 단계

첫번째 스펙으로 테스트 코드를 작성합니다.

[ClickCounter.spec.js]

```javascript
describe('ClickCounter  모듈', ()=> {
  describe('getCount()', ()=> {
    it('초기값이 0인 카운터 값을 반환한다', ()=> {
      const counter = App.ClickCounter();
      expect(counter.getCount()).toBe(0);
    })
  })
})
```

테스트 코드를 작성 후 테스트를 실행하면 테스트에 실패합니다. 실제 구현된 기능 코드가 없기 때문인다. 



TDD 2단계  - 성공 단계

이제 테스트를 통과하도록 기능 코드, 모듈에 코드를 만들어 봅니다.

[ClickCounter.js]

```javascript
var App = App || {}

App.ClickCounter = () => {
  return {
    getCount() {
      return 0
    }
  }
}
```

이제 테스트를 실행하면 테스트에 성공합니다. 



TDD 3단계 - 리팩토링 단계

카운터 값은 버튼을 클릭할 때마다 변경 될 것이기 때문에 상수 값을 사용하지 않고 변수로 뺍니다.

```javascript
App.ClickCounter = () => {
		let count = 0
    return {
        getCount() {
            return count
        }
    }
}
```

이렇게 안심하고 리팩토링 할 수 있는 이유는 이미 작성한 테스트 코드 때문입니다. 리팩토링 한 다음에 올바르게 동작 한다는 것을 보장하기 위해서는 테스트들 돌려보면 바로 알 수 있습니다.



### Spec 2. ClickCounter 모듈의 increase()는 카운터 값을 1만큼 증가한다.

TDD 1단계 - 실패 단계

[ClickCounter.spec.js]

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

TDD 2단계  - 성공 단계

[ClickCounter.js]

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



TDD 3단계 - 리팩토링 단계

중복된 테스트 코드가 보이므로 리팩토링 해봅니다.
자스민의 beforeEach 함수는 it 함수 호출 직전에 실행됩니다.

```javascript
describe(()=> {
	beforeEach(()=> { //실행순서 1
	afterEach(()=>{   //실행순서 3
	it(()=>{          //실행순서 2     
```

물론 beforeEach, AfterEach를 사용하지 않으면 it 함수만 실행 됩니다.

[ClickCounter.spec.js]

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

하지만 이 경우 초기 값이 0 이 아닐 경우 increase() 실행 결과가 1이 아닐 수 있습니다.

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



## ClickCountView 모듈 (보이는 부분)

이번에는 화면에 보이는 부분을 만드어 봅니다. 클릭 카운터 모듈에서 만든 카운터 데이터를 DOM에 반영에서 화면에 보이게 하는 것이 이 모듈의 역할입니다. 데이터를 출력할 뿐 아니라 클릭 이벤트 핸들러는 바인딩 하는 것도 담당합니다.

### Spec 1. ClickCountView 모듈의 updateView()는 카운트 값을 출력한다.

그런데.. 데이터를 조회활 ClickCounter를 어떻게 얻을 수 있을까요? 게다가 데이터를 출력할 Dom 엘리먼트는 어떻게 테스트 할 수 있을까요?

이럴 경우 ***주입***을 하면 됩니다.

- ClickCounter 객체를 만들어서 ClickCounteView 모듈의 생성인자로 전달합니다.
- 데이터를 출력할 Dom 엘리먼트도 같은 방법으로 인자로 전달 합니다.



TDD로 개발을 하게 되면 이런 의존 관계이 있는 모듈을 주입하는 경향이 생깁니다. 주입된 객체들은 하나의 역할만 수행하기 때문에 단일 책임의 원칙도 지킬 수 있습니다.



TDD 1단계 - 실패 단계

[ClickCountView.spec.js]

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



TDD 2단계  - 성공 단계

[ClickCountView.js] 

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



ClickCountView에 의존성 주입이 되어있는지는 어떻게 체크할까요? 모듈을 사용하는 측에서 의존 모듈을 넘겨주지 않으면 ClickCountView 모듈은 제대로 동작하지 않을 수 있습니다.

자스민 프레임웍에는 toThrowError라는 매처 함수가 있습니다. expect 안에 예외를 던지는 함수를 전달을 하면 toThrowError 함수로 에러 여부를 확인할 수 있습니다.



TDD 1단계 - 실패 단계

[ClickCountView.spec.js]

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



TDD 2단계  - 성공 단계

[ClickCountView.js] 

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

 

### Spec 2. ClickCountView 모듈의 increaseAndUpdateView() 는 카운트 값을 증가 시키고 그 값을 출력한다.

카운트 값을 증가 시킨다는 건 결국 ClickCounter 모듈의 increase 함수가 실행되었다는 뜻이고, 그 값을 출력한다는 것은 ClickCountView 모듈의 updateView 함수가 실행 되었다는 뜻입니다. 

### 테스트 더블

TDD 용어 중에 테스트 더블이라는 개념이 있습니다.

단위테스트의 패턴으로 테스트 하기 곤란한 컴포넌트를 대체 하는 것을 말한다. 함수를 테스트 하기 위해서 어떤 행동만 흉내내는 것이다.

아래 5가지를 통칭해 테스트 더블 이라고 합니다.

- 더미 - 함수 인자를 채우기 위해 사용한다.
- 스텁 - 더미를 발전한 것으로 리터값이 있는 것을 말한다.
- 스파이 - 스텁과 유사하지만 내부적으로 계속 기록을 남긴다.
- 페이크 - 스텁이 리턴 값이 하드코딩 되있다면 페이크는 실제 값을 리턴한다.
- 목 - 더미, 스텁, 스파이를 혼합한 형태를 말한다.



자스민에서는 테스트 더블을 스파이스(spices)라고 부릅니다.

spyOn() 함수를 이용해서 스파이스를 만들어 봅니다.

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

즉 bar() 함수가 MyApp.foo() 함수를 실행하는지 검증하는 코드입니다.



TDD 1단계 - 실패 단계

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



TDD 2단계  - 성공 단계

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



### Spec 3. 클릭 이벤트가 발생하면 increaseAndUpdateView를 실행한다.

여기에서도 주입 패턴을 사용합니다. 카운터 값을 출력할 Dom 엘리먼트를(updateElement)를 주입 했듯이 클릭 이벤트 핸들러 (increaseAndUpdateView)를 바인딩 할 Dom 엘리먼트 (triggerElement)도 주입 해 줍니다.



TDD 1단계 - 실패 단계

[ClickCountView.spec.js]

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



TDD 2단계  - 성공 단계

[ClickCountView.js] 

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

테스트 코드로 요구사항을 만들고 이것을 기반으로 모듈 로직을 만들어내는 과정을 경험습니다. 처음에는 매우 느릴 수 밖에 없습니다. 이런 과정이 견고한 로직을 만들어내게 되고 오히려 개발 속도가 빨라지게 됩니다. 익숙해지냐의 문제이므로 스스로 이런 과정을 반복하면서 익숙해지는 수 밖에 없습니다.

# 뭐가 개선 됐을까?

<기존 코드>

```javascript
<button onClick="counter++; countDisplay()">증가</button>
<span id="counter-display">0</span>

<script>
var counter = 0;

function countDisplay(){
	var el = document.getElementById('counter-display');
	el.innerHTML = counter;
}
</script>
```



<변경 된 코드>

관심사 분리, 전역 변수 사용하지 않음, 재사용성 증가

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



# 추가 요구 사항도 쉽게 받을 수 있는 코드 만들기

### spec 3. ClickCounter 모듈은 데이터를 주입 받는다.

요구사항은 매번 변하기 마련입니다. 증가 버튼 뿐 아니라 감소 버튼도 필요 하거나 증가 버튼을 클릭하면 2씩 증가해야 될지도 모릅니다.

증가 버튼 뿐 아니라 감소 버튼을 추가하려면 어떻게 해야할까요? 버튼도 두 개 있어야 하고 이에 상응 하는 클릭 카운터도 두 개 있어야 한다. 증가는 increase, 감소는 decrease 함수를 사용해야 합니다.

그런데 클릭 카운터는 데이터를 클로즈 변수로 간직하고 있기 때문에 모듈 간에 공유를 할 수 가 없습니다. 증가를 위한 클릭 카운터와 감소를 위한 클릭 카운터를 따로 만들게 되면 각자 독립적인 카운터 밸류를 가지는 상황이 발생하기 때문에.. 현재 하나의 카운터 값만 출력해서 이것을 증가 또는 감소해야 하는 상황과는 맞지 않습니다.

그렇기 때문에 카운터 모듈은 데이터를 주입 받도록 합니다. 공통의 데이터가 있고 클릭 카운터 객체가 그 공통의 객체를 주입 받게 만듭니다.



TDD 1단계 - 실패 단계

[ClickCounter.spec.js]

```javascript
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
```



TDD 2단계  - 성공 단계

[ClickCounter.js]

```javascript
var App = App || {}

App.ClickCounter = _data => {
  if(!_data) throw Error('_data')

  const data = _data;

  /*
  데이터를 공유하기 위해서는 기존처럼 정수 같은 원시형을 사용 할 수 없다.
  생성 인자로 넘겨주면 그 값은 복사되기 때문이다.
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
```

[ClickCounterView.spec.js]

```javascript
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
```



### spec 4. ClickCounter 모듈의 increase 함수는 대체 될 수 있다.

increase 함수는 올릴 수도 있지만 내릴 수도 있어야 합니다. (이름 변경 : increase -> count)

TDD 1단계 - 실패 단계

[ClickCounter.spec.js]

```javascript
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
```



TDD 2단계  - 성공 단계

[ClickCounter.js]

```javascript
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
```

### 정리

```javascript
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
```



ClickCounter 모듈과 ClickCountView 모듈은 쉽게 변경되지 않으면서도 다양한 요구사항을 처리 할 수 있는 아주 유연한 코드가 되었습니다. 

프론트엔드 개발에 있어서 TDD를 이용하는 방법을 살펴 보았습니다. 이런 과정을 반복해서 몸에 익히는 것이 견고한 소프트웨어를 만들 수 있는 방법입니다.

