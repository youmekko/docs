# 바벨

브라우저마다 사용하는 언어가 달라서 프론트엔드 코드는 일관적이지 못할 때가 많다. 스펙과 브라우저가 개선되고 있지만 여전히 익스플로러는 프라미스를 이해하지 못한다. 작년까지만 해도 사파리 최신 브라우저는 Promise.prototype.finally 메소드를 사용할 수 없었다. 프론트엔드 개발에서 크로스브라우징 이슈는 코드의 일관성을 해치고 초심자를 불안하게 만든다.

크로스 브라우징의 혼란을 해결해 줄 수 있는 것이 바벨이다. ECMAScript2015+ 로 작성한 코드를 모든 브라우저에서 동작하도록 호환성을 지켜준다. 타입스크립트, JSX처럼 다른 언어로 분류되는 것도 포함한다.

# 바벨의 기본 동작

```bash
npm install -D @babel/core @bable/cli
```

바벨을 설치하고 나면 `node_modules/.bin/babel` 폴더에 추가된 바벨 명령을 사용 할 수 있다.

app.js

```jsx
const alert = msg => window.alert(msg);
```

npx를 사용하면 설치한 모듈을 바로 실행 할 수 있다.

```bash
npx babel app.js
```

바벨은 세 단계로 빌드를 진행한다.

1. 파싱 (Parsing)
2. 변환 (Transforming)
3. 출력 (Printing)

코드를 읽고 추상 구문 트리(AST)로 변환하는 단계를 **'파싱'** 이라고 한다. 이것은 빌드 작업을 처리하기에 적합한 자료구조 인데 컴파일러 이론에서 사용되는 개념이다. 추상 구문 트리를 변경하는 것이 **'변환'** 단계이다. 실제로 코드를 변경하는 작업을 한다. 변경된 결과물을 **'출력'**  하는 것을 마지막으로 바벨은 작업을 완료한다.

그런데 위 바벨 실행의 결과를 보면 변한게 없다.

(결과)

```jsx
const alert = msg => window.alert(msg);
```

# 플러그인

바벨은 파싱과 출력망 담당하고 '변환(트랜스포밍)' 은 플러그인이 담당한다.

# 커스텀 플러그인

플러그인을 직접 만들면서 동작의 원리를 살펴보자.

my-babe-plugin.js (babel 사이트 예제 코드)

```javascript
module.exports = function myBabelPlugin() {
	return {
		visitor : {
			Identifier(path) {
				const name = path.node.name;
				
				//바벨이 만든 AST 노드를 출력한다.
				console.log('Identifier() name:', name)

				//변환 작업
				path.node.name = name
					.split("")
					.reverse()
					.join("")
			}
		}
	}
}
npx babel app.js --plugins './my-babel-plugin.js'
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7fcab66-5ea7-4683-9ef0-d424c9d14a97/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7fcab66-5ea7-4683-9ef0-d424c9d14a97/Untitled.png)

이번에는 es6의 const variable을 var로 변경하는 플러그인을 만들어 보자.

```javascript
module.exports = function myBabelPlugin() {
	return {
		visitor : {
			VariableDeclation(path) {
				console.log("VariableDeclaration() kind', path.node.kind); //const
				//const => var 변환
				if (path.node.kind === 'const') {
					path.node.kind = 'var'
				}
			}
		}
	}
}
```

다시 해당 플러그인으로 실행을 하면 아래처럼 const가 var로 변경된다.

```javascript
var alert = msg => window.alert(msg);
```

# 플러그인 사용하기

위 같은 결과를 만드는 것이 block-scoping 플러그인이다. const, let 처럼 블록 스코핑을 따르는 예약어를 함수 스코핑을 사용하는 var로 변경한다.

```javascript
npm install -D @babel/plugin-transform-block-scoping
npx babel app.js --plugins @babel/plugin-transform-block-scoping
var alert = msg => window.alert(msg);
```

결과는 동일 하다.

이번에는 arrow 함수를 바꿔보자. arrow-functions 플러그인을 이용해서 일반 함수로 변경할 수 있다.

```javascript
npm install -D @babel/plugin-transform-arrow-funtions
npx babel app.js --plugins @babel/plugin-transform-block-scoping --plugins @babel/plugin-transform-arrow-funtions
var alert = function(msg) {
	window.alert(msg);
}
```

ECMAScript5에서부터 지원하는 엄격 모드를 사용하는 것이 안전하기 때문에 'use strict' 구문을 추가하는 strict-mode 플러그인을 사용해보자.

```javascript
npm install -D @babel/plugin-transform-strict-mode
npx babel app.js --plugins @babel/plugin-transform-block-scoping --plugins @babel/plugin-transform-arrow-funtions --plugins @babel/plugin-transform-strict-mode 
'use strict'
var alert = function(msg) {
	window.alert(msg);
}
```

커맨드라인 명령어가 점점 길어지기 때문에 설정 파일로 분리하는 것이 낫겠다. webpack.config.js를 기본 설정 파일로 사용하듯 바벨도 babel.config.js를 사용한다.

프로젝트 루트에 babel.config.js를 만들어 보자.

```javascript
//babel.config.js
module.exports = {
	plugins : [
		"@babel/plugin-transform-block-scoping",
		"@babel/plugin-transform-arrow-funtions",
		"@babel/plugin-transform-strict-mode"
	]
}
npx babel app.js
'use strict'
var alert = function(msg) {
	window.alert(msg);
}
```

그런데 ECMA2015+ 로 코딩을 할 때 필요한 플러그인을 일일이 설정하는 것은 무척 지난 일이다. 목적에 맞게 여러가지 플러그인을 세트로 모아 놓은 것을 프리셋 이라고 한다.

# 커스텀 프리셋

위에서 사용한 세 개의 플러그인을 하나의 프리셋으로 만들어보자.

```javascript
//my-babel-preset.js
module.exports = function mypreset() {
	return {
		plugins: [
			"@babel/plugin-transform-block-scoping",
		  "@babel/plugin-transform-arrow-funtions",
		  "@babel/plugin-transform-strict-mode"
		]
	}
}
```

프리셋을 사용하기 위해서는 bable.config.js를 수정한다.

```javascript
module.exports = {
	preset: [
		'./my-babel-preset.js'
	]
}
npx babel app.js
'use strict'
var alert = function(msg) {
	window.alert(msg);
}
```

# 바벨 사용법과 웹팩 통합

바벨을 목적에 따라 몇 가지 프리셋을 제공한다.

- preset-env
- preset-flow
- preset-react
- preset-typscript

### preset-env

ECMAScript2015+를 변환할 떄 사용한다. 바벨 7 이전 버전에는 연도별로 각 프리셋(babel-reset-es2015, babel-reset-es2016 등... )을 제공했지만 지금은 env 하나로 합쳐졌다.

```bash
npm install -D @babel/preset-env
```

babel.config.js를 수정한다.

```javascript
module.exports = {
	presets : [
		'@babel/preset-env'	
	]
}
npx babel app.js
```

### 타켓 브라우저

프리셋을 사용할 때 이 프리셋을 사용할 떄 특정 브라우저를 지원하라고 설정을 할 수 있다.

```javascript
//babel.config.js
module.exports = {
	presets : [
		['@babel/preset-env', {
			targets: {
				chrome: '79',
				ie: '11'
			}
		}]
	]
}
```

[caniuse.com](http://caniuse.com) 이라는 사이트에서 문법 지원에 관련된 내용을 확인 할 수 있다.

### 폴리필

```javascript
//app.js
new Promise();
//babel.config.js
module.exports = {
	presets : [
		['@babel/preset-env', {
			targets: {
				chrome: '79',
				ie: '11'
			}
		}]
	]
}
npx babel app.js
new Promise()
```

위 코드를 실행하면 타켓 브라우저에 ie 11을 설정 했기 때문에 플러그인이 프라미스를 ECMA2015+ 버전으로 변환할 것을 기대했지만 예상과 다르다.

바벨은 ECMAScript2015+를 ECMAScript5 버전으로 변환 할 수 있는 것만 빌드한다. 그렇지 못한 것들은 '폴리필' 이라고 부르는 코드 조각을 추가해서 해결한다.

(프라미스는 ECMAScript5 버전으로 대체할 수 없다. 다만 ECMAScript2015 버전으로 구현할 수는 있다. 참고 core-js promise, 최근에는 core-js를 많이 사용하는 추세이다.)

env 프리셋은 폴리필을 지정 할 수 있는 옵션을 제공한다.

```javascript
//babel.config.js
module.exports = {
	presets : [
		['@babel/preset-env', {
			targets: {
				chrome: '79',
				ie: '11'
			},
			useBuildIns: 'usage' //'entry', false
			core-js: {
				version:2 //최신버전은 3
			}
		}]
	]
}
```

### 웹팩으로 통합하는 방법

실무 환경에서는 바벨을 직접 사용하는 것보다 웹팩으로 통합해서 사용하는 것이 일반적이다. 로더형태로 제공하는데 babel-loader가 그것이다.

```javascript
npm install -D babel-loader
npm i core-js@2
```

웹팩 설정에 로더를 추가한다.

```javascript
//webpack.config.js
{
	test: /\\.js$/,
	loader: 'babel-loader',
	exclude: /node_modules
}
```

### 정리

바벨은 일관적인 방식으로 코딩하면서 다양한 브라우저에서 돌아가는 어플리케이션을 만들기 위한 도구이다.

바벨의 코어는 파싱과 출력만 담당하고 변환 작업은 플러그인이 처리한다.

여거래의 플러그인들을 모아놓은 세트를 프리셋이라고 하는데 ECMAScript+ 환경은 env 프리셋을 사용한다.

바빌에 변환하지 못하는 코드는 폴리필이라고 부르는 코드조각을 불러와 결과물에 로딩해서 핵결한다.

babel0-loader로 웹팩과 함께 사용하면 훨씬 단순하고 자동화된 프론트엔드 개발 환경을 갖출 수 있다.

### 바벨 실습

```javascript
//webpack.config.js
... {
	test: /\\.js$/,
	use: ["babel-loader"],
	exclude: /node_modules/
},
module.exports = {
	presets: [
		["@babel/preset-env", {
			targets: {
				ie: "11"
			},
			useBuiltIns: "usage",
			corejs: {
				version: 2
			}
		}]
	]
}
```



