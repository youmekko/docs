# 실용적인 프런트엔드 테스팅 전략

*연구주제를 TDD로 정했을 무렵 마침 Naver Toast 에서 프론트엔드 테스트와 관련해서 세미나를 진행한다는 글을 보았습니다. ([https://meetup.toast.com/posts/221](https://meetup.toast.com/posts/221)) 신청했지만.. 인원 제한으로 추첨에 떨어져서 다녀오지 못했는데 마침 동영상이 올라왔길래 내부 공유용으로 정리해 보았습니다.*

*개인적으로 저의 이전 글을 보고 이 동영상을 보시면 조금 더 이해가 잘 됩니다!*

*동영상 링크 [https://www.youtube.com/watch?v=q9d631Nl0_4&feature=youtu.be](https://www.youtube.com/watch?v=q9d631Nl0_4&feature=youtu.be)*

근본적으로 개발자가 왜 테스트를 작성해야 할까요?

# Confidence

- 내가 뭔가 잘못 건드리면 꼭 알려줘!
- 니가 괜찮다고 했으니, 이제 퇴근할게!
- 라이브러리 업데이트 완료! 문제 없는거지?
- 이 기능을 추가하려면 구조를 바꿔야 하는데 너만 믿고 한번 해볼게
- 성능은 향상 되었는데, 기능에는 문제 없겠찌?
- 자, 이제 이 버그는 다시 볼 일 없겠지?

# 내가 작성한 프런트엔드 테스드트는 왜~?

테스트라는 것은 기본적으로 입력 값과 출력 값의 검증이다.

그렇게 봤을 때 백엔드는 입력 값과 출력 값을 데이터로 검증이 가능하다. 

프론트엔드의 경우는 입력 값을 데이터라고 보기에는 애매하고 사용자의 액션에 따른 것이 대부분이다. 출력 값은 더 애매한데 출력 값은 사실상 화면이 바뀌는 것이다.

근본적인 어려움의 원인은 입출력을 데이터로서 코드로 검증하기 힘들다는 것이다.

# 프론트엔드의 입력과 출력

사실 입력 데이터는 아래의 것들로 다 생성이 가능하다.  강조하고 싶은 것은 출력이다. 

## 입력 데이터

- DOM 이벤트 : 마우스, 키보드, 터치 등의 입력 이벤트
- 생성 : 브라우저의 이벤트 시뮬레이션 API 사용
- 라우팅 I/O : URL 변경, 네트워크/로컬 파일, 로컬 스토리지/쿠키
- 생성 :  브라우저 API 목킹 / E2E 테스트 도구 사용

## 출력 데이터(시각적 요소)

- 코드관점 : HTML, CSS
- 검증 : 생성된 HTML, CSS 코드의 내용을 비교
- 사용자관점 : 브라우저가 렌더링한 화면(픽셀 정보)
- 검증 : 브라우저가 렌더링한 화면을 캡처해서 이미지로 비교

# 시각적 요소의 검증 (예제) - 카운터

```javascript
function Counter() {
	const [count, setCount] = userState(0);
	const increase = () => setCount(count + 1);
	const decrease() = () => setCount(count - 1);

	return (
		<div className="simple-counter">
			<button className="dec" onClick={decrease}>-</button>
			<span class="value">{count}</span>
			<button className="dec" onClick={decrease}>-</button>
		</div>
}
```

## 1. HTML 비교

dom구조니까 innerHTML 같은 걸 써서 HTML 문자열로 비교하면 된다. 

하지만 문자열로 비교할 때는 인덴트나 띄어쓰기 등이 문제가 될 수 있기 때문에 diffableHTML 같은 라이브러리를 써서 예쁘게 포맷팅을 해주고 비교할 수 있다.

```javascript
it('생성시 버튼과 초기값을 렌더링 한다', () => {
	const div = document.createElement('div');
  ReactDom.render(<Counter />, div);

	expect(diffableHTML(div.innerHTML)).toBe(
		deffableHTML(`
			<div className="simple-counter">
				<button className="dec">-</button>
				<span class="value">0</span>
				<button className="dec">-</button>
			</div>
	 `)
	)
});
```

## 2. 스냅샷 테스트

JEST 같은 도구가 생기면서 스냅샷 테스트를 유행 시켰다.  굳이 예측 되는 문자열을 입력할 필요 없이 toMatchSnapShot() 이라는 함수를 실행하면 된다. 처음 실행 할 때 결과 값을 파일로 저장 해 뒀다가 두 번째 실행 할 때부터는 이전에 실행 한 데이터와 파일을 비교해서, 달라진 부분이 있으면 에러를 뱉어내거나, 의도된 변화라 생각하면 스냅샷을 갱신하거나 할 수 있다.

```javascript
it('생성시 버튼과 초기값을 렌더링 한다', () => {
	const div = document.createElement('div');
	ReactDom.render(<Counter />, div);
	
	expect(diffableHTML(div.innerHTML))
		.toMatchSnapShot()
});

exports[`생성시 버튼과 초기값을 렌더링 한다. 1`] = `
"
<div className=\\"simple-counter\\">
	<button className=\\"dec\\">-</button>
	-
	<span class=\\"value\\">
	0
	</span>
	<button className=\\"dec\\">
	-
	</button>
</div>
"
`;
```

## HTML 비교의 문제점

과연 이 테스트가 처음에 얘기했던 신뢰와 자신감(Confidence)을 나에게 줄 수 있을가?

- HTML 구조를 보고 실제 결과물 (픽셀 정보)를 예측 할 수 있는가?
- 테스트가 성공하면 항상 의도된 결과가 나온다고 보장 할 수 있는가?
- HTML/CSS를 리팩토링 할 때 테스트가 도움을 주는가?

결과적으로 이런 방식의 테스트는 신뢰를 주지 못한다.

## 구현 상세 테스트 VS 동작 테스트

- 구현 상세 (Implementation detail)에 대한 테스트를 지양하고, 동작 (Behavior)를 테스트 하라.
- private 메서드에 대한 테스트를 지양하고, public 인터페이스를 테스트하라.
- 구현 상세(private) : HTML, CSS
- 동작 (public) : 픽셀 정보

## 3. 이미지 비교

지금까지는 픽셀 정보로 테스트를 많이 안했을까? 도구의 발전이 미비했기 때문이다. 지금은 도구가 많이 발전했다.

시각적 회귀 테스트(Visual Regression Testing)

회귀 테스트라고 부르는 이유는 psd나 스케치 파일로 디자인 시안을 받아서 그대로 던졌을 때 비교해주는 수준은 안되기 때문이다. 다만 처음 렌더링 한 결과를 눈으로 확인 한 다음에 그것을 스크린샷으로 저장하고 그 다음 다시 실행을 했을 때 다시 실행된 스크린 샷과 비교를 해서 다른 점을 알려주는 용도로 사용할 수는 있기 때문이다.

의도된 변화라고 하면 스크린샷을 갱신하면 되고, 아니라면 의도치 않은 변화가 생긴 것이니 버그를 수정하면 된다.

하지만 이 방식도 여전히 한계가 있다. 

### 캡처 이미지의 신뢰성

- 픽셀 단위 비교 : 의미 있는 변경 점을 찾기 어려움
- 운영 체제, 브라우저 등의 렌더링 방식 차이 (픽셀 정보라는 것은 육안으로 보기에는 구분이 안가더라도 브라우저 렌더링 방식에 따라서 천차 만별일 수 있다. 그래서 픽셀 단위 테스트를 신뢰하기 어렵다. )
- 이미지/폰트 로딩시간, 커서, 애니메이션 등으로 인한 캡처 시점 차이

### 결과 확인 및 이력 관리

- 커맨드 라인에서 확인 불가능 → 결과 확인을 위한 UI 필요
- 브라우저, 뷰 포트 크기 등의 케이스별 이미지 파일 생성, 관리
- 테스트 실행 단위별 이미지 파일 히스토리 관리

해결이 불가능한 이슈는 아니지만 배보다 배꼽이 꺼질 수 있다.

# 시각적 테스트 전문 도구

## Visual Testing as a Service (applitools, percy, chromatic 등)

설치형 보다는 서비스 형태로 많이 제공한다. 다양한 환경을 직접 내부에서 관리하면서 실행을 해줘야 하기 때문이다.  테스트 코드를 작성하고 스크립트를 실행하면 결과를 받아서 브라우저 별로 스크린샷을 찍어서 비교를 해준다. 

- 캡처 이미지 안정화 (캡처 시점 조절, 애니메이션 정지 등)
- 의미 있는 차이를 분석해서 화면에 표시 (머신 러닝 등 사용)
- 다양한 브라우저 실행 환경 제공
- 브라우저 및 뷰포트 사이즈별 이미지 생성, 관리
- 병렬 실행을 통한 테스트 속도 향상
- 다양한 테스트 도구 연동 (StoryBook, Seleninum, Cypress)
- 다양한 CI 환경과 연동 (Jenkins, Travis, CricleCI)

단점은 유료라는 것인데.. 가격은 효과에 비하면 감내할 수 있는 수준이다.

### 시각적 테스트 도구 사용 예제 (percy)

![Untitled/Untitled.png](Untitled/Untitled.png)

(잠깐! puppeteer라는 걸 쓰고 있는데.. chrome브라우저를 nodejs api로 조작할 수 있게 만들어 주는 라이브러리다.)

![Untitled/Untitled%201.png](Untitled/Untitled%201.png)

![Untitled/Untitled%202.png](Untitled/Untitled%202.png)

# 시각적 테스트 for Everything?

### 테스트 환경 / 실행 속도

- 스크린 샷을 생성할 수 있는 환경(Selenium, Puppeteer 등) 에서만 테스트 가능
- 테트스 실행 속도가 느림 → 빠른 피드백을 받을 수 없음 → 개발 속도 저하

### 테스트 문서화 기능

- 테스트의 의도가 코드에 명확하게 드러나지 않음 → (문서화 기능 X)
- TDD 불가능 → 회기 테스트 용도로만 사용

### 단일 테스트에 영향을 주는 요소

- 시각적 요소 : 레이아웃 변경, 폰트 변경, 색상 변경 등
- 기능적 요소 : 사용자 입력 처리, 데이터 연산, 상태(state) 변경
- 테스트가 실패 했을 때 원인을 파악하기 어려움

# 시각적 테스트 VS 기능적 테스트

이 두 개를 잘 구분하는 것이 효율적인 테스트 코드를 작성하는 키 포인트이다!

![Untitled/Untitled%201.png](Untitled/grid1.png)

### 기능적 테스트 예제 (이 부분은 앞전의 강의와 유사하네요!)

기능적 테스트를 작성할 때는 시각적 요소를 완벽하게 분리해야 한다. (시각적 요소 의존성 제거)

- DOM 탐색 : 잘못된 예

    ~~~javascript
    container.querySelector('.box > .value') //부모 > 자식 선택자 사용
    container.querySelector('span.value') //태그 선택자 사용
    constainer.querySelector('text.write') //시각적 표현을 위한 클래스명 사용
    container.children[0] //children, parent 등의 구조 속성 이용
    ~~~

- DOM 탐색 : 올바른 예

    ~~~javascript
    cotainer.querySelector('.count-value') //기능만을 위한 의미 있는 클래스명
    container.querySelector('[data-testid="count-value"]') //테스트를 위한 별도의 속상 값 사용
    ~~~

- data-testid 사용 예제

    ~~~javascript
    function Counter() {
     //.. 
    	return ( 
    			<div className="simple-counter">
    			-
    			{count}
    			-
    		</div>
    	);
    
    }

    const getByTest = (node, id) => 
    	node.querySelector([data-testid="${id}"]);
    
    ~~~
    
    ```javascript
    it('초기값은 0이다', () => {
    	const value = getByTestId(container, 'value');
    	expect(value.textContent).toBe('0)';
    });
    
    it('+ 버튼 클릭스 1 증가한다.', () => {
    	const btnIcn = getByTestId(container, 'btn-inc');
    	const value = getByTestId(container, 'value');
    	
    	btnInc.click();
    	expect(value.textContent).toBe('1');
    ```
    });

이런식으로 시각적 요소를 잘 떼어놓으면 시각적 테스트에서는 기능 테스트를 할 필요가 없다.

# Story Book (UI 컴포넌트 개발 환경)

테스트 도구라기보다는 개발 도구에 가깝다. 

### 독립된 컴포넌트 개발 환경

- 전체 애플리케이션에서 컴포넌트를 분리해서 독립된 개발 환경 제공
- 통합도니 상태의 환경 보다 훨씬 빠른 피드백을 받으며 개발 가능
- 특히 시각적 요소 개발(HTML/CSS)에 특화된 개발 환경

### 컴포넌트의 다양한 상태를 등록/관리

- 특정한 상태의 컴포넌트를 미리 등록해 놓고 재사용
- 컴포넌트의 다양한 시각적 상태를 빠르게 파악 할 수 있음
- 재현하기 어려운 복잡한 상태를 빠르게 재현 후 개발, 수정 가능

## Story 작성

```javascript
import React from 'react'
import Counter from '../src/Counter';

export default { title : 'Simple Counter' }

export const singleDigit = () => <Counter initValue={8} />;
export const doubleDigit = () => <Counter initValue={75} />;
export const trippleDigit = () => <Counter initValue={325} />;
```

![Untitled/Untitled%203.png](Untitled/Untitled%203.png)

## StoryBook ! == 시각적 테스트

### 시각적 요소 개발 환경

- 시각적 요소 개발을 위한 분리된 단위 테스트 환경
- 특히 HTML/CSS 개발 단계에서 빠른 피드백 제공

### 시각적 테스트 자동화를 위한 보조 도구

- 검증이 필요한 다양한 시각적 케이스를 최소한의 코드로 관리
- 수동으로 테스트 할 때에도 많은 시간과 비용을 절약
- 자동화 도구를 도입할 때 추가 비용 없이 사용 가능

Storybook은 테스트 도구는 아니지만 테스트에 굉장히 많은 도움을 준다. 도구를 사용해 자동화하지 않고 수동으로 테스트 할 때에도 미리 원하는 케이스를 등록해 놓았기 때문에 클릭클릭해가며 눈으로 확인하기가 쉽다. 그리고 대부분의 시각적 테스트 도구들은 Storybook을 다 지원한다.

## Naver Toast에서 사용하고 있는 Stroybook 예제 - 캡쳐 파일

(좋아보여서 같이 보려고 캡처했습니다.)

Toast의 그리드에서 사용하고 있는 예제라고 합니다. 그리드에서 발생 가능한 다양한 시각적 케이스를 전부 등록해서 쓰고 있습니다. 시각적 케이스는 설명이 필요한 부분이 있기 때문에 우측의 Notes 기능도 사용하고 있습니다. 

![Untitled/Untitled%204.png](Untitled/Untitled%204.png)

applitools 는 오픈소스인 경우 무료 계정을 준다.

![Untitled/Untitled%205.png](Untitled/Untitled%205.png)

## Storybook의 활용 범위

### 디자인 / 기획 부서와의 커뮤니케이션

- 디자인 QA를 위한 구현 결과물 공유
- 구현 이슈나 프로토타이핑 결과를 공유하고 피드백 반영
- 디자인 시스템 가이드 문서

### 개발자 사이의 커뮤니케이션

- 공통 컴포넌트의 사용법 가이드 및 API 문서로 활용
- 예제 페이지/ 튜토리얼 등의 문서로 활용

(예제)

![Untitled/Untitled%206.png](Untitled/Untitled%206.png)

결제 실패 페이지 등은 결제를 실패하기 전까지는 절대 볼수 없는 화면 등을 등록하고 사용하기에도 유용하다.

# 단위 테스트 VS 통합 테스트

## 카운터 예제 서버 연동하기

redux / react-redux / redux - thnk

![Untitled/Untitled%207.png](Untitled/Untitled%207.png)

이런식으로 작성하고 redux 홈페이지에 가서 보면 테스트를 모듈 별로 따로따로 하라고 되어있다.

## 서버 연동 테스트 : 컴포넌트

내가 상태를 관리하지 않고 Prop을 받았으니까, Prop을 받아쓰도록 만들고 클릭 했을 때도 니가 받아온 Prop을 실행하는지만 테스트 해라.

```javascript
export function Counter ({ value, update}) {
	const increase = () => update('inc');
	const decrease = () => update('dec');

	return (
		<div className="simple-counter">
		..
		</div>
	);
}

it("+ 클릭 시 update('inc')를 호출한다", ()=> {
	const updateSpy = jset.fn();
	render(<Counter value={10} update={updateSpy} />);
	
	const btnInc = getByTestId(container, 'btn-inc');
	btnInc.click();
	
	expect(updateSpy).toHaveBeenCalledWidth('inc');
});
```

## 서버 연동 테스트 : 액션 생성자

```javascript
export const UPDATE_REQUEST = 'UPDATE_REQUEST';
export const UPDATE_SUCCESS = 'UPDATE_SUCCESS';

export function updateRequest(value) {
	return { type : UPDATE_REQUEST, value }
}

export function updateSucess(value) {
	return { type: UPDATE_SUCCESS, value} 
}

it('UPDATE_REQUEST 액션 생성', () => {
	expect(updateRequest(10)).toEqual({
		type: UPDATE_REQUEST,
		value: 10
	})
});

it('UPDATE_SUCCESS 액션 생성', () => {
	expect(updateSuccess(11).toEqaul({
		type: UPDATE_SUCCESS,
		value: 11
	})
});
```

## 서버 연동 테스트 : 비동기 액션 (thunk)

```javascript
export function update(type) {
	return async (dispatch, getState) => {
		const currentValue = getState();
		dispatch(updateRequest(currentValue));
		
		const { data : { value } } = 
			await axios.put(`/${type}`);
		
		dispatch(updateSuccess(value));
	};
}

it("update ('inc') 비동기 액션", async () => {
	const middlewares = [ thunk ];
	const mockStore = configureMockStore(middlewares);
	const mockAxios = new MockAdapter(Axios);

	mockAxios.onPut('/inc').reply(200, { value : 11 });

	const store = mockStore(10);
	await store.dispatch(update('inc'));

	expect(store.getActions()).toEqual([
		{ type : UPDATE_REQUEST, value : 10 },
		{ type : UPDATE_REQUEST, value : 11 }
	]);
});
```

이렇게 했을 때 문제점은 기존 코드가 간단한데에 비해.. 테스트 코드가 너무 길다! 왜냐면 목킹 같은걸 하기 때문이다. 

더 중요한 것은 개별 모듈로 테스트 한 경우는 모듈의 연결 부위는 테스트가 되지 않은 것이다. 목킹을 다 해놓았기 때문이다. 

![Untitled/Untitled%208.png](Untitled/Untitled%208.png)

![Untitled/Untitled%209.png](Untitled/Untitled%209.png)

# Why Most Unit Testing is waste (James O Coplien)

## 테스트는 비용이다.

- 테스트 코드는 작성 및 유지보수, 컴퓨팅 시간, 자원 등의 비용이 든다.
- 불필요한 테스트를 최소화 해야 한다.

## 단위 테스트는 필수가 아니다.

- 복잡한 로직이 없는 (trivial) 코드의 단위 테스트는 동어반복적(tautological)(불필요하게 같은 뜻의 말을 표현만 달리 하여 되풀이하는 것) 이다.
- 단위 테스트는 지엽적인 검증이며, 사업적 가치를 갖지 않는 경우가 많다.
- 시스템 테스트와 중복된느 단위 테스트는 제거하는 것이 좋다.
- 핵심 알고리즘을 갖는 모듈에 대한 단위 테스트는 여전히 중요하다.

## 단위 테스트

단위 테스트와 프로덕션 코드가 1:1 매칭 되지는 않는다.

![Untitled/Untitled%2010.png](Untitled/Untitled%2010.png)

## Sociable vs Solitary (Martain Fowler)

Sociable Test / Solitary Test

![Untitled/Untitled%2011.png](Untitled/Untitled%2011.png)

즉, 단위테스트와 통합테스트라는 것이 단순히 모듈 단위로 분리한다 분리하지 않는다의 의미가 아니라 단위를 어떤 식으로 나누냐의 문제라는 것이다.

# 컴포넌트 단위의 통합 테스트

![Untitled/Untitled%2012.png](Untitled/Untitled%2012.png)

컴포넌트 기반의 개발을 할때 가장 효율적인 테스트는 이 컴포넌트가 실제로 하는 동작을 다 묶어서 하나의 단위로 보는 것이라고 생각합니다. 

## 서버 연동 테스트 : 통합 테스트

서버 목킹 하는 것 외에는 실제 코드를 그대로 쓰고 있다.

```javascript
it('+ 버튼을 클릭하면 1 증가한다', async () => {
	const mockAxios = new MockAdapter(axios);
	mockAxios.onPut('/inc').reply(200, { value : 11 });

  render(<Provider store={store}><Counter /></Provider>);

  const btnInc = getByTestId(container, 'btn-inc');
	const value = getByTestId(constainer, 'value');

	await wait(() => {
		expect(value.textContent).toBe('11');
	})
});
```

## 테스트 커버리지 (통합 테스트)

실제 코드를 그대로 써서 한바퀴 돌았으니 커버리지도 전부 높아졌다.

![Untitled/Untitled%2013.png](Untitled/Untitled%2013.png)

많은 경우에 컴포넌트 단위로 통합 테스트를 하는 것이 훨씬 효율적이다.

# Cypress (통합/E2E 테스트 도구)

E2E 테스트 도구라고 볼 수 있으나 브라우저 내에서 돌아가는 것이다. 전통적인 E2E 테스트 도구로 Selenium을 생각하는 경우가 많다음

(아직 조금 남음)