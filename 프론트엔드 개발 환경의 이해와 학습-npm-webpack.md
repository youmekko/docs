# 프론트엔드 개발 환경의 이해와 학습

# 프론트엔드 개발에 Node.js가 필요한 이유

### 최신 스펙으로 개발 할 수 있다.

webpack, babel, npm 등은 노드 위에서 돌아간다.

Typescript, SASS 같은 고수준 프로그래밍 언어도 전용 트랜스파일러가 필요한데 이것 역시 Node.js 환경이 뒷받침 되어야만 프론트엔드 개발 환경을 만들 수 있다.

### 빌드 자동화

파일을 압축, 코딩을 난독화 하고, 브라우저에 따라 폴리필을 추가하는 등 개발 이외에 후속 작업을 거친 후 배포를 하게 된다. Node.js는 이런 일련의 빌드 과정을 이해하는데 큰 역할을 한다. 뿐만 아니라 라이브러리 의존성을 해결하고 각종 테스트를 자동화 하는데도 사용한다.

### 개발 환경 커스터마이징

vue-cli나 CRA(create react app)등을 사용할 수 없는 경우도 빈번하다. 커스터마이징 하려먼 Node.js 지식이 필요하다. 

# Node.js 설치

- LTS 버전과 최신 버전을 제공하는데 node를 서버로 구성하는 경우는 신중 해야겠지만 개발 환경의 경우 최신 버전을 써 보자.
- REPL (read-eval-print loop)
- node를 설치하면 npm도 설치 된다.
- `npm init` 프로젝트를 생성하는 명령어로 프로젝트의 메타 정보를 입력할 수 있다.
- 터미널에서 npm 을 입력하면 사용 가능한 npm 커맨드들을 확인 할 수 있다. 기본적으로 제공하는 커맨드 외에 커맨드를 추가하고 싶다면  package.json에 추가하면 된다. 커스텀 커맨드를 실행할 때는 `npm run 커맨드 이름]` 을 입력하면 된다.

# 외부 패키지를 관리하는 방법

### 1. CDN(contents delivery network)

보통 JS 라이브러리, CSS 라이브러리를 빠르게 제공해주는 서버이다. 

(예) react cdn

    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>

하지만 CDN 서버 장애로 인해 외부 라이브러리를 사용할 수 없다면 우리가 만드는 어플리케이션도 정상적으로 동작하지 않을 것이다.

### 2. 직접 다운로드 하는 방법

CDN과 달리 독립적으로 웹 어플리케이션을 제공할 수 있다.

하지만 라이브러리는 계속 업데이트 될 것이고 우리가 만드는 프로젝트에서도 최신 버전으로 매번 교체해야 한다. 이것은 매우 귀찮은 일이고 호환성 확인까지 하려면 실수 할 여지가 많다.

### 3. NPM을 이용하는 방법

node.js 가 패지키 버전을 관리하는 방식

프로젝트에서 사용하는 패키지의 버전을 매우 엄격하게 제한한다면 프로젝트를 버전업 하는데 꽤 힘들 수 있다. 사용하는 패키지를 전부 버전업 해야 하기 때문이다.

반대로 느슨하게 풀어놓으면 문제가 해결될까? 오히려 여러 버전 별로 코드를 관리해야 하는 혼란스러움을 겪게 될 것이다.

**유의적 버전** (Sementic Version)

- Major Version - 기존 버전과 호환 되지 않게 변경한 경우
- Minor Version - 기존 버전과 호환 되면서 기능이 추가 된 경우
- Patch Version - 기존 버전과 호환 되면서 버그를 수정한 경우

Node.js는 유의적 버전의 규칙을 따른다.

```javascritp
//특정 버전 사용
1.2.3

//특정 버전보다 높거나 낮을 경우
>1.2.3
>=1.2.3
<1.2.3
<=1.2.3

//~(틸트) 마이너 버전이 명시되어 있으면 패치 버전을 변경한다.
~1.2.3 //1.2.3부터 1.3.0 미만까지를 포함한다. 
~0 //마이너 버전이 없으면 마이너 버전을 갱신한다. 0.0.0부터 1.0.0까지를 포함한다.

//^(캐럿) 정식 버전에서 마이너와 패치버전을 변경한다.
^1.2.3 //1.2.3부터 2.0.0 미만까지를 포함한다. 
^0 //정식버전 미만인  0.x버전은 패치만 갱신한다. 0.0.0부터 0.1.0 미만까지를 포함한다.
```


npm으로 패키지를 설치하면 package.json에 설치한 버전을 기록하는데 캐럿 방식을 이용한다. 초기에는 틸트를 사용하다가 캐럿을 도입해서 기본 동작으로 사용했다. 

# 웹팩이 필요한 이유와 기본 동작

### 배경 - 모듈

JS에서 문법 수준의 모듈을 지원하기 시작한 것은 ES2015부터다. import/export 구문이 없었던 모듈 이전 상황을 살펴 보는 것이 웹팩 등장 배경을 이해하는데 수월 하다.

과거에는 하나의 html 파일에 여러 개의 js를 올리고 다른 파일에 있는 함수를 호출해서 사용을 했었다. 하지만 그렇게 되면 전역 스코프가 오염이 된다. 하나의 파일에 있는 함수는 해당 파일 안에서만 유효한 것이 아니라 어플리케이션의 전역(window)에 등록되기 때문이다. (window) 전역 스코프가 오염되면 예측할 수 없는 어플리케이션이 되어 런타임 에러가 발생한다.

**IIFE**

이런 문제를 해결하기 위해서 IIFE (Immediately Invoked function express) 방식의 모듈을 사용한다. 즉시 함수 실행은 함수 안에 독립적인 스코프가 생기기 때문에 함수 내부에서 정의한 것은 함수 외부에서 접근 할 수 없기 때문에 전역 스코프가 오염되는 것을 예방할 수 있다.

이러한 방식으로 JS 모듈을 구현하는 대표적인 명세가 AMD와 CommonJS 다. 

**CommonJS**

CommonJS는 JS를 사용하는 모든 환경에서 모듈을 사용하는 것이 목표다. exports 키워드로 모듈을 만들고 require() 함수로 불러 들이는 방식이다. 대표적으로 Node.js에서 사용한다.

```javascript
exports function sum (a, b) {
	return a + b
}

const sum = require('./math.js');
sum(1, 2); //3
```

브라우저처럼 외부에서 자바스크립트를 로딩 해야 하는 비동기 환경에서는 AMD(Asynchronous Module Definition) 를 사용한다.

CommonJS와 AMD를 통합해서 지원하는 것이 UMD(Universal Module Definition) 이다.

이렇게 각 커뮤니티에서 각자 스펙을 제안하다가 ES2015에서 표준 모듈 시스템을 내놓았다. 지금은 바벨과 웹팩을 이용해서 이 표준 모듈 시스템을 사용하는 것이 일반적이다. 

```js
export function sum (a, b) {
	return a + b;
}

import * as math from './math.js';

math.sum(1, 2); //3
```

### 브라우저의 모듈 지원

모든 브라우저들이 모듈을 지원 하는 것은 아니다. IE를 비롯한 몇 몇 브라우저에서는 여전히 모듈을 사용하지 못한다. 크롬은 버전 61부터 모듈 시스템을 지원한다.

 [크롬] 크롬에서는 script 태그를 로딩할 때 `type="text/javascript"` 대신 `type="module"` 을 사용한다. 

```js
<script type="module" src="app.js"></script>
```

모든 브라우저에서 무관하게 모듈을 사용하고 싶다면 이제야 웹팩이 나올 차례이다.

# mode/entry/output(path, filename)

번들 작업을 하는 webpack 패키지와 웹팩을 터미널 명령으로 사용할 수 있는 webpack-cli를 설치하자.

```shell
npm install -d webpack webpack-cli
```

`node_module/.bin/webpack --help` 로 일단 한번 살펴보자.

웹팩을 실행 할 때는 필수적인 옵션이 3개가 있다.

- `--mode` : development, production, none
- `--entry` : 모듈의 시작점
- `--output` : entry 를 통해서 번들링을 하고 그 결과를 저장하는 경로를 설정하는 옵션이다.

이 외에도 많은 옵션이 있다. 매번 명령어를 입력 할 때마다 옵션을 타이핑 할 수 없으니 웹팩 설정 파일을 만들어 보자.

- `--config` : 웹팩 설정 파일을 지정 할 수 있다. 기본은 파일명은 webpack.config.js 또는 webpackfile.js 다.

webpack.config.js

```js
const path = require('path');

//여기서 module.exports는 ES6 모듈이 아니라 node의 모듈이다.
module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        //out diretory 명으로 절대 경로로 입력한다.
        path: path.resolve('./dist'),
        //name은 entry의 main으로 치환된다. 이렇게 하면 output이름을 동적으로 만들어 낼 수 있는 효과가 있다.
        filename: '[name].js'
    }
}
```

npm 은 프로젝트를 관리하는 도구이다. 특히 스크립트를 자동화 하는 기능이 있다. 

package.json

```json
{
  "name": "lecture-frontend-development-environment",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build" : "webpack" //이렇게만 해도 기본 config 파일인 webpack.config.js 파일을 읽어서 번들링 한다
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11"
  }
}
```

# 로더

웹팩은 모든 파일을 모듈로 바라본다. JS 모듈 뿐 아니라 CSS, 이미지, 폰트 까지 모두 모듈로 보기 때문에 ES6의 import 키워드를 사용하면 자바스크립트 코드 안으로 가져올 수 있다.

이것이 가능한 이유는 웹팩에 로더가 있기 때문이다.

웹팩에서 로더의 역할을 모든 파일을 자바스크립트의 모듈처럼 만들어 주는 것이다. 

### 커스텀 로더 만들어 보기

 my-webpack-loader.js

```js
//로더는 함수 형태로 작성한다.
module.exports = function myWebpackLoader (content) {
    console.log('myWebpackLoader works');
    return content;
}
```

이렇게 만든 로더를 사용해 하려면 webpack.config.js 파일로 가보자.

```js
const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
		//로더는 모듈 객체의 rules라는 배열에 추가 할 수 있다.
    module : {
        rules : [
            {
                test : /\.js$/, //로더가 처리해야 할 파일들의 패턴을 입력한다. 여기서는 모든 JS 파일.
                use: [
                    path.resolve('./my-webpack-loader.js')
                ]
            }
        ]
    }
}
```

정리 하면 웹팩의 로더는 각 파일을 처리하기 위한 것이다. 처리 할 파일의 패턴을 명시하고 패턴에 해당 하는 파일들을 로더 함수를 이용해 처리한다.

# 자주 사용하는 로더

### css-loader

css에 로더를 설정하게 되면 자바스크립트에서 css파일을 모듈로 불러 올 수 있다.

~~~shell
npm install -d css-loader
~~~

```js

import './app.css'

body {
	background-color: green;
}

const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
		//로더는 모듈 객체의 rules라는 배열에 추가 할 수 있다.
    module : {
        rules : [
            {
                test : /\.css$/,
                use: [
                    'css-loader'
                ]
            }
        ]
    }
}
```

HTML이 코드가 DOM이라는 모습으로 변환 되어야 브라우저에서 문서가 보이듯이 CSS 코드도 CSS OM이라는 형태로 바뀌어야만 브라우저에서 모습을 드러낸다.

그렇게 하려면 css 코드를 직접 불러오거나 인라인 script로 넣어 줘야 한다. 위에서는 아직 그러한 처리를 하지 않고 자바스크립트 파일에만 css 코드가 있어서 브라우저에 반영되지 않는 것이다.

그래서 나온 것이 style-loader이다. 자바스크립트로 변경 된 스타일 코드를 HTML에 넣어주는 역할을 한다. 

 CSS 코드를 모듈로 사용하고 번들을 하려면 css-loader와 style-loader를 같이 사용해야 한다.

### style-loader

~~~shell
npm instll -d style-loader
~~~

```js

const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
		//로더는 모듈 객체의 rules라는 배열에 추가 할 수 있다.
    module : {
        rules : [
            {
                test : /\.css$/, //로더가 처리해야 할 파일들의 패턴을 입력한다. 여기서는 모든 JS 파일.
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
}
```

번들링 후 html을 확인 하면 아래처럼 head에 style 태그가 들어가 있다.

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
		<style>body {
		    background-color: green;
		}</style>
</head>
```

### file-loader

CSS 뿐만 아니라 이미지 파일도 처리할 수 있다.

~~~shell
npm install -d file-loader
~~~

```js
body {
    background-image: url(bg.png)
}

const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
		//로더는 모듈 객체의 rules라는 배열에 추가 할 수 있다.
    module : {
        rules : [
            {
                test: /\.css$/, 
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }, {
                test: /\.png$/,
                use: [
                    'file-loader'
                ]
            }
        ]
    }
}
```

웹팩은 빌드를 할 때마다 유니크한 값(해시)을 생성하는데, 이것으로 파일명이 바뀐다. 캐시 갱신을 위해서다. 정적 파일의 경우 브라우저에서 캐시하는 경우가 하나인데, 성능 개선을 위해서다. 파일 내용이 달라 지고 이름이 같으면, 이전의 캐시로 저장했던 파일 내용을 브라우저가 사용하는데 이런 것을 예방하는 방법 중 하나가 파일 이름을 변경 하는 것이다.

webpack.config.js 파일을 아래처럼 수정해 보자.

```js
const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
		//로더는 모듈 객체의 rules라는 배열에 추가 할 수 있다.
    module : {
        rules : [
            {
                test: /\.css$/, //로더가 처리해야 할 파일들의 패턴을 입력한다. 여기서는 모든 JS 파일.
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }, {
                test: /\.png$/,
                loader: 'file-loader',
                options: {
                    //파일 로더가 사용하는 파일을 모듈로 사용 했을 때, 경로 앞에 추가되는 문자열이다. 
                    publicPath: './dist',
                    //파일 로더가 파일을 아웃풋에 복사할 때 사용하는 파일 이름이다. 원본파일.확장자?해시 (캐시 무력화를 위해서 해시를 쿼리스크링으로 사용한다.)
                    name: '[name].[ext]?[hash]'
                }
            }
        ]
    }
}
```

### url-loader

사용하는 이미지 갯수가 많아지면 네트워크 리소스를 사용하는 부담이 있고 사이트 성능에 영향을 줄 수도 있다. 만약 한 페이지 안에서 작은 이미지 여러 개를 사용한다면 Data URI Schema를 이용하는 방법이 더 낫다. 

**Data URI Schema**

```html
<img src="data:image/png;base64,iVBORw0KGgoAAA
ANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4
//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU
5ErkJggg==" alt="Red dot" />
```

이렇게 하면 네트워크 통신 없이 화면을 그려 준다.

url-loader는 이러한 처리를 자동화 해주는 기능을 한다.

~~~shell
npm install -d url-loader
~~~

```js
import './app.css'
import nyancat from './nyancat.jpg'

document.addEventListener('DOMContentLoaded', () => {
    document.body.innerHTML = `
        <img src="${nyancat}" />
    `
})

const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
    module : {
        rules : [
            {
                test: /\.css$/, 
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }, {
                test: /\.(png|jpg|gif|svg)$/,
                loader: 'file-loader',
                options: {
                    publicPath: './dist',
                    name: '[name].[ext]?[hash]'
                }
            }
        ]
    }
}
```

기본적으로 file-loader 로도 처리할 수 있다.

하지만 파일이 아주 작은 경우 파일로 옮길 필요 없이 base64로 인코딩해서 넣어 보자.

```js
const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js'
    },
    output: {
        path: path.resolve('./dist'),
        filename: '[name].js'
    },
		//로더는 모듈 객체의 rules라는 배열에 추가 할 수 있다.
    module : {
        rules : [
            {
                test: /\.css$/, //로더가 처리해야 할 파일들의 패턴을 입력한다. 여기서는 모든 JS 파일.
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }, {
                test: /\.(png|jpg|gif|svg)$/,
                loader: 'url-loader',
                options: {
                    publicPath: './dist',
                    name: '[name].[ext]?[hash]',
										//20kb 미만의 파일은 url-loader를 사용하고 이상은 file-loader를 이용한다.
										//20kb 미만의 파일은 base64로 변환해 자바스크립트 문자열로 변환하고 그 이상은 파일을 복사한다.
                    limit: 20000 //20kb
                }
            }
        ]
    }
}
```

### 정리

- css-loader : css 파일을 자바스크립트 모듈처럼 사용할 수 있도록 처리하는 역할을 한다.
- style-loader : css-loader에서 처리된 자바스크립트 문자열로 되어 있는 스타일시트 코드를 html에 주입시켜서 브라우저에  스타일이 적용되도록 하는 역할을 한다.
- file-loader: 이미지 파일을 모듈로 사용할 수 있도록 변환하는 역할을 한다. 사용할 파일을 아웃풋 경로로 이동 시키는 역할을 한다.
- url-loader : 파일을 base64로 인코딩 해서 그 결과를 자바스크립트 문자열로 변환하는 역할을 한다. 처리할 파일의 크기 제한을 둬서 일정 파일만 미만만 변환을 하고 나머지 파일을 file-loader로 위임하는 역할을 한다.

# 플러그인

웹팩에서 마지막으로 알아야 될 기본 개념이 플러그인이다. 로더가 파일 단위의 처리 였다면 플러그인은 번들 된 결과물을 처리한다. 번들된 자바스크립트를 난독화 한다거나 특정 텍스트를 추출하는 용도로 사용한다.

### 커스텀 플러그인

로더가 함수로 정의했던 것과 다르게 플러그인은 클래스로 정의한다. 

```js
class MyPlugin {
    apply (compiler) {
        compiler.hooks.done.tap('My Plugin', stat => {
            console.log('MyPlugin: done');
        })
    }
}

module.exports = MyPlugin;
```

플러그인이 어떻게 번들 된 결과물에 접근할 수 있을까? 웹팩 내장 코드 중에 BannerPlugin 코드를 참고하자.

```js
class MyPlugin {
    apply (compiler) {
        // compiler.hooks.done.tap('My Plugin', stat => {
        //     console.log('MyPlugin: done');
        // })

        compiler.plugin('emit', (compilation, callback) => {
            const source = compilation.assets['main.js'].source();
            compilation.assets['main.js'].source = () => {
                const banner = [
                    '/**',
                    '* 이것은 Banner Plugin이 처리한 결과 입니다',
                    '* Build date : 2020-03-11',
                    '*/'
                ].join('\n')
                return banner + '\n\n' + source;
            }
            callback();
        })
    }
}

module.exports = MyPlugin;
```

이렇게 하면 빌드된 파일에 banner를 추가할 수 있다.

실제로 플러그인을 직접 작성할 일은 거의 없다. 웹팩에서 자주 사용하는 플러그인을 사용하거나 써드파티 라이브러리를 찾아 사용한다.

# 자주 사용하는 플러그인

### BannerPlugin (웹팩기본)

빌드 결과 물에 빌드 정보나 커밋 버전 같은 걸 추가 할 수 있다.

```js
const webpack = require('webpack');
//터미널 명령어를 실행할 수 있는 노드 모듈
const childProcess = require('child_process');

module.exports = {
    ...
    plugins: [
        new webpack.BannerPlugin({
            banner: `
                Build Data : ${new Date().toLocaleString()}
                Commit Version : ${childProcess.execSync('git rev-parse --short HEAD')}
                Author: ${childProcess.execSync('git config user.name')}
            `
        })
    ]
}
```

빌드 결과 물에 배너 정보를 추가하는 이유는.. 빌드 → 배포 후 실제 정적 파일들이 잘 배포 되었는지..? 캐시에 의서 갱신되지 않았는지 확인하는 용도로 쓴다.

### DefinePlugin (웹팩기본)

프론트엔드 소스 코드는 개발 환경과 운영 환경으로 나누어서 운영을 한다. 환경에 따라서 api 주소도 다를 수 있다. 배포 할 때마다 코드를 수정하면 아무래도 휴먼 에러, 장애가 발생하기 쉽다. 따라서 api주소처럼 환경 의존적인 정보를 소스가 아닌 다른 곳에서 관리하는 것이 좋다.

이러한 환경 변수들을 어플리케이션에 제공하기 위해서 definePlugin 을 많이 쓴다.

```js
const webpack = require('webpack');

module.exports = {
    ...
    plugins: [
        new webpack.DefinePlugin({
            TWO: '1+1',
            THREE: JSON.stringify('1+2'),
						'api.domain' : JSON.stringify('http://dev.api.domain.com')
        })
    ]
}
```

DefinedPlugin이 기본적으로 어플리케이션에 주입 해 주는 코드가 있는데 노드의 환경 변수이다. `process.env.NODE_ENV` 

이 외에도 웹팩 컴파일 시간에 결정 되는 값을 상수 문자열로 어플리케이션에 주입 할 수 있다.

app.js에서 콘솔로 확인 해 보자.

```js
import './app.css'
import nyancat from './nyancat.jpg'

document.addEventListener('DOMContentLoaded', () => {
    document.body.innerHTML = `
        <img src="${nyancat}" />
    `
})

console.log(process.env.NODE_ENV);//콘솔에 webpack config 파일의 mode가 출력된다. (ex: 'development')
console.log(TWO); //2
console.log(THREE); //1+2
console.log(api.domain); //http://dev.api.domain.com
```

빌드 타임에 결정 된 값을 어플리케이션에 전달할 때는 이 플러그인을 사용 해 보자.

### HtmlWebpackPlugin (써드파티 패키지)

HTML 파일을 후처리 하는데 사용한다. 웹팩으로 빌드 한 결과물을 자동으로 로딩하는 코드를  HTML에 주입해 준다.

```js
npm install html-webpack-plugin

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    ...
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ]
}
```

이렇게 HtmlWebpaackPlugin 을 쓰면 빌드 과정에 HTML 도 포함하기 때문에 좀 더 의존적이지 않은 코드를 만들 수 있다.

또한 좀 더 유동적인 HTML(템플릿) 파일을 만들 수 있다.

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
		<!-- ejs 템플릿 문법 -->
    <title>Document<%= env %></title>
</head>
<body>
</body>
</html>
~~~

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    ...
    plugins: [
       new HtmlWebpackPlugin({
            template: './src/index.html',
            templateParameters: {
                env: process.env.NODE_ENV === "development" ? '(개발용)' : ''
            }
        })
    ]
}
```

~~~shell
NODE_ENV=development npm run build
~~~



뿐만 아니라 HTML 압축하고 주석을 제거하는 기능도 있다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
		<!-- 이것은 주석 입니다 -->
</head>
<body>
</body>
</html>
```

~~~js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    ...
    new HtmlWebpackPlugin({
            template: './src/index.html',
            templateParameters: {
                env: process.env.NODE_ENV === "development" ? '(개발용)' : ''
            },
            minify:  process.env.NODE_ENV === "production" ? {
                collapseWhitespace: true, //빈칸 다 없애기
                removeComments: true //주석 제거
            } : false
        })
}
~~~



### CleanWebpackPlugin (써드파티 패키지)

아웃풋 폴더를 삭제해 주는 플러그인이다. (빌드 이전 결과물을 제거). 실습하면서 필요할 때마다 아웃풋 폴더를 직접 삭제 했는데, 이 플러그인을 사용하면 빌드 할 때마다 삭제 해준다.

~~~shell
npm install clean-webpack-plugin
~~~

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    ...
    plugins: [
        new CleanWebpackPlugin()
    ]
}
```

### MiniCssExtractPlugin (써드파티 패키지)

스타일 시트가 많아지면 하나의 자바스크립트 결과물로 만드는 것이 부담 일 수 있다. 번들 결과에서 css 코드만 따로 뽑아서 css 파일을 별도로 만들어 역할에 따라 파일을 분리하는 것이 좋다. 브라우저에서는 큰 파일 하나를 로딩하는 것보다 여러 개의 작은 파일 동시에 로딩하는 것이 빠르기 때문이다.

~~~shell
npm install mini-css-extract-plugin
~~~



```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    module: {
			rules: [
				{
	        test: /\.css$/,
	        use: [
							//다른 플러그인들과 다르게 제공하는 로더를 추가해야 한다.
	            process.env.NODE_ENV === "production"
	            ? MiniCssExtractPlugin.loader
	            : 'style-loader','css-loader'
	        ]
        }
			]
		},
    plugins: [
				//이  플러그인은 자바스크립트에서 css를 뽑아내는 과정을 추가하는 것이기 때문에 굳이 개발환경에서는 하지 않는 것이 개발 환경에서 조금 더 빨리 빌드 할 수 있다.
        new CleanWebpackPlugin(),
        ...(process.env.NODE_ENV === "production" 
        ? [new MiniCssExtractPlugin({filename: '[name].css'})] 
        : []
        )
    ]
}
```

아웃풋 폴더에 css 파일이 추가되고 html 에도 css 파일 로딩이 추가 된다.

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width,initial-scale=1">
		<title>Document</title>
		<link href="main.css" rel="stylesheet">
	</head>
	<body>
		<script type="text/javascript" src="main.js">
		</script>
	</body>
</html>
```

### 정리

- BannerPlugin : 번들링 된 결과물 상단에 빌드 정보를 추가하는 역할
- DefinePlugin : 빌드 타임에 결정 되는 환경 변수를 어플리케이션 단에 주입하기 위해 사용한다. 대표적으로 Api 주소 등이 있다.
- HtmlWebpackPlugin : html을 빌드 과정에 추가 하고 동적으로 추가 되는 js와 css를 그리고 빌드 타임에 결정되는 값들을 HTML파일을 동적을 만드는데 사용한다.
- CleanWebpackPlugin : 빌드 할 때마다 아웃풋 폴더를 삭제 해주는 역할을 한다.
- MiniCssExtractPlugin : 번들 된 JS 코드에서  CSS만 뽑아내서 CSS파일을 만드는 플러그인이다.

# 웹팩 개발 서버

지금까지는 브라우저에서 파일을 직접 로딩해서 결과물을 확인했다. 인터넷에 웹사이트를 게시하려면 서버 프로그램으로 이 파일을 읽고 요청한 클라이언트에게 제공해야 한다.

개발 환경에서도 이와 유사한 환경을 갖추는 것이 좋다. 운영 환경과 맞춤으로써 배포 시 잠재적 문제를 미리 확일 할 수 있다. 게다가 ajax 방식의 api 연동은 cors 정책 때문에 반드시 서버가 필요하다.

프론트엔드 개발 환경에서 이러한 개발용 서버를 제공 해주는 것이 webpack-dev-server 다.

~~~shell
npm i -d webpack-dev-server
~~~

```js
"scripts": {
    "start": "webpack-dev-server" 
}
```

### 기본 설정

웹팩 설정 파일의 devServer객체에 개발 서버 옵션을 설정 할 수 있다.

```js
module.exports = {
	devSever: {
		contentBase: path.join(__dirname, "dist"),
		publicPath: "/",
		host: "dev.domain.com",
		overlay: true,
		port: 8081,
		stats: "errors-only",
		historyApiFallBack: true
	}
}
```

- contentBase : 정적 파일을 제공한 경로로 기본 값은 웹팩 아웃풋이다.
- publicPath : 브라우저를 통해 접근하는 경로로 기본 값은 '/' 경로 이다.
- host : 개발 환경에서 도메인을 맞춰야 하는 상황에 사용한다. 예를 들어 쿠키 기반의 인증은 인증 서버와 동일한 도메인으로 개발 환경을 맞춰야 한다. 운영 체제의 호스트 파일에 해당 도메인과 127.0.0.1 연결을 추가한 뒤 host 속성에도 도메인을 설정해서 사용한다.
- overlay : 빌드 시 에러나 경고를 브라우저 화면에 표시한다.
- port : 개발 서버 포트 번호를 설정한다. 기본 값이 8080.
- stats: 메세지 수준을 조정할 수 있다. 'none', 'errors-only', 'minimal', 'normal', 'verbose' 로 메세지 수준을 조절한다.
- historyApiFallBack: 히스토리 API를 사용하는 SPA 개발 시 설정한다.  404가 발생하면 index.html로 리다이렉트 한다.

이 외에도 개발 서버를 실행 할 때 명령어 인자로 `--progress`  를 넣어주면 빌드 상태를 볼 수 있다.

# API 서버 연동

### 목업 API 1 (devServer.before)

before 함수를 이용해서 목업 API 를 만들 수 있다.

```js
module.exports = {
    ...
    devServer: {
        overlay: true,
        stats: 'errors-only',
        before: app => {
            app.get('/api/users', (req, res) => {
                res.json([{
                    "id": 1,
                    "name": "Alice" 
                 }, {
                     "id": 2,
                     "name": "Bek"
                 }, {
                     "id": 3,
                     "name": "Chris"
                 }])
            })
        }
    }
		...
}
```

~~~js
import './app.css'
import axios from 'axios'

document.addEventListener('DOMContentLoaded', () => {

    const res = await axios.get('./api/users');
    console.log(res)
    
    document.body.innerHTML = (res.data || [])
        .map(user => {
            return `<div>${user.id} : ${user.name}</div>`
        }).join("")
})
~~~



### 목업 API 2

목업 API 가 많이 필요할 땐 connect-api-mocker 패키지의 도움을 받자. 특정 목업 폴더를 만들어 api 응답을 담은 파일을 저장한 뒤 이 폴더를 api로 제공한다.

~~~shell
npm i -d connect-api-mocker
~~~

```js
[{
    "id": 1,
    "name": "Alice" 
 }, {
     "id": 2,
     "name": "Bek"
 }, {
     "id": 3,
     "name": "Chris"
 }]
```

~~~js
const apiMocker = require('connect-api-mocker')

module.exports = {
	...
	devServer: {
	        overlay: true,
	        stats: 'errors-only',
	        before: app => {
	            app.use(apiMocker('/api', 'mocks/api'));
	        }
	}
	...
}
~~~



### 실제 API 연동

CORS 를 해결하는 방법은 두가지 이다.

**서버에서 해결** 

해당 api 응답헤더에 'Access-Control-Allow-Origin' 헤더를 추가한 뒤 응답하면 브라우저에서 응답 데이터를 받을 수 있다.

**프론트에서 해결**

서버 응답 헤더를 추가 할 필요 없이 웹팩 개발 서버에서 api서버로 프록싱 하는 것이다. 웹팩 개발 서버는 proxy속성으로 이를 지원한다.


```js
module.exports = {
  ...
  devServer: {
    overlay: true,
    stats: "errors-only",
    proxy: {
        '/api' : {
					//내 경우 localhost가 아니라 [::1]로 써줘야 제대로 동작했다.
          target: 'http://[::1]:8081'
        }
    }
  },
  ...
};
```

프론트에서 api 요청을 하면 웹팩 서버가 프록시 설정으로 이미 셋팅 되어 있는 api서버에 요청을 보내고 그 응답을 다시 프론트로 보내준다.

# 핫로딩 (핫모듈리플레이스먼트)

웹팩 서버를 셋팅하면, 파일이 변화하면 브라우저가 자동으로 갱신 되게 하는 기능이 추가 됐다. 

핫모듈리플레이스먼트는 전체 화면을 새로고침 하지 않고 변경된 모듈만 화면에서 갱신하는 것이다. 조금 더 개발을 편리하게 할 수 있다.

```js
module.exports = {
    ...
    devServer: {
        overlay: true,
        stats: 'errors-only',
        proxy: {
            '/api' : 'http://[::1]:8081' 
        },
        hot: true
    }
		...
}
```

HotModuleReplacement(이하 HMR)을 제대로 사용하려면 HMR 인터페이스를 맞춰줘야 한다.

```js
if (module.hot) {
	module.hot.accpet('./result', () => {
		//result 모듈이 변경 되었을 때, 이 콜백이 실행 된다.
		console.log('result module chanaged')
	})
}
```

(예)

```js
if (module.hot) {
	module.hot.accpet('./result', async () => {
		//result 모듈이 변경 되었을 때, 이 콜백이 실행 된다.
		resultEl.innerHTML = await result.render();
	})
}
```

### 핫로딩을 지원하는 로더

이러한 HMR 인터페이스를 구현한 로더만이 핫 로딩을 지원하는데 대표적인 것이 웹팩 기본 편에서 보았던 style-loader, file-loader 다.

vue나 react를 사용하고 있으면 알맞은 핫로딩을 사용하면 된다.

# 최적화

코드가 많아지면 번들링 된 결과물도 커지기 마련이다. 거의 메가바이트로 커질 수도 있는데 브라우저 성능에 영향을 줄 수도 있다. 

어떻게 웹팩으로 번들 결과를 압축하고 상황에 따라 작은 파일 여러개로 분리할 수 있는지 알아보자.

### mode 설정

웹팩에 내장 되어 있는 최적화 방법 중 mode 값을 설정하는 방식이 가장 기본이다. 세 가지 값이 올수 있는데 지금까지 설정한 'development' 는 디버깅 편의를 위해 아래 두 개 플러그인을 사용한다.

- NamedChunksPlugin
- NamedModulesPlugin

DefinedPlugin을 사용한다면 process.env.NODE_ENV 값이  'development' 로 설정되어 어플리케이션에 전역 변수로 주입된다.

반면 모드를 'prodction'으로 설정하면 자바스크립트 결과물을 최소화 하기 위해 다음 일곱개 플러그인을 사용한다.

- FlagDependencyUsagePlugin
- FlagIncludedChunksPlugin
- ModuleConcatenationPlugin
- NoEmitOnErrosPlugin
- OccurenceOrderPlugin
- SideEffectsFlagPlugin
- TerserPlugin

DefinedPlugin을 사용한다면 procee.env.NODE_ENV 값이 'production'으로 설정 되어 어플레키에션 전역 변수로 들어간다.

결국 환경변수 NODE_ENV 값에 따라 모드를 설정하도록 웹팩 설정 코드를 다음과 같이 추가할 수 있다.

```js
const mode = process.env.NODE_ENV || 'development'
module.exports = {
	mode,
}
```

### optimazation 속성으로 최적화

빌드 과정을 커스터마이징 할 수 있는 여지를 제공하는데 그것이 바로 optimazation속성이다.

HtmlWebpackPlugin이 html 파일을 압축한 것처럼 css 파일도 빈칸을 없애는 압축을 하려먼 optimize-css-assets-webpack-plugin을 사용하면 된다.

```js
npm i -d optimize-css-assets-webpack-plugin

const OptimizeCSSAseetsPlugin = require('optimize-css-assets-webpack-plugin')

module.exports = {
	//일반 플러그인과 달리 plugins가 아니라 optimization 아래에 추가한다.
	optimization: {
		minimizer: mode === 'production' ? [new OptimizeCSSAseetsPlugin()] : []
	}
}
```

 mode=production일 경우 사용 되는 TerserWebpackPlugin은 자바스크립트 코드를 난독화 하고 debugger 구문을 제거한다. 기본 설정 외에도 콘솔 로그를 제거하는 옵션도 있는데 배포 버전에는 로그를 감추는 것이 좋을 수도 있기 때문이다.

```shell
npm i -D terser-webpack-plugin
```

optimization 배열에 추가한다.

```js
const OptimizeCSSAseetsPlugin = require('optimize-css-assets-webpack-plugin')
const TerserWebpackPlugin = require('terser-webpack-plugin')

module.exports = {
	//일반 플러그인과 달리 plugins가 아니라 optimization 아래에 추가한다.
	optimization: {
		minimizer: mode === 'production' 
			? [new OptimizeCSSAseetsPlugin(), 
				 new TerserWebpackPlugin({
						terserOption: {
							compress: { 
								drop_console: true //콘솔 로그를 제거한다.
							}
						}
				 })] 
			: []
	}
}
```

### 코드 분할

코드를 압축하는 것 외에도 아예 결과물을 여러 개로 쪼개면 좀 더 브라우저 다운로드 속도를 높일 수 있다. 큰 파일 하나를 다운로드 하는 것보다 작은 파일 여러 개를 동시에 다운로드 하는 것이 더 빠르기 때문이다.

가장 단순한 것은 엔트리를 여러 개로 분리하는 것이다.

```js
module.exports = {
	...
	entry: {
		main: './src/app.js',
		result: './src/result.js'
}
```

하지만 빌드 된 두 파일을 비교해 보면 중복된 코드가 있을 수 있다.

SplitChunksPlugin은 코드를 분리할 때 중복을 예방하는 플러그 인이다. optization.splitChunks 속성을 설정하는 방식이다.

```js
const OptimizeCSSAseetsPlugin = require('optimize-css-assets-webpack-plugin')
const TerserWebpackPlugin = require('terser-webpack-plugin')

module.exports = {
	//일반 플러그인과 달리 plugins가 아니라 optimization 아래에 추가한다.
	optimization: {
		minimizer: mode === 'production' 
			? [new OptimizeCSSAseetsPlugin(), 
				 new TerserWebpackPlugin({
						terserOption: {
							compress: { 
								drop_console: true //콘솔 로그를 제거한다.
							}
						}
				 })] 
			: [],
		//이렇게 하면 여러개의 entry 포인트에서 중복된 코드를 제거하고 빌드한다.
		//중복코드는 vendors~main~result.js
		splitChunks: {
			chunks: 'all'
		}
	}
}
```

엔트리포인트를 분리하는 것은 개발자가 직접 적절하게 분리해야 하기 때문에 손이 많이 가는 편이다. 이것을 자동으로 변경해 주는 방식이 있는데 웹팩의 Dynamic Import(동적 임포트)다.

### 다이나믹 임포트

기존의 코드는 아래와 같았다. import ~ from으로 controller라는 모듈을 가져와서 사용했다.

```js
import controller from './controller'

document.addEventListener('DomContentLoaded', () => {
	controller.init(document.querySelector('#app'))
})
```

이를 동적으로 import 하려면 다음처럼 변경한다.

```js
function getController() {
	return import (/* webpackChunkName: "controller" */ '.controller').then(m => 
		return m.default
	})
}

document.addEventListener('DomContentLoaded', () => {
	getController().then(controller => {
		controller.init(document.querySelector('#app'))
	})
})
```

getController() 라는 함수로 모듈을 가지고 왔다. import()함수로 가져올 컨트롤러 모듈 경로를 전달하는데 주석으로 webpackChunkName: "controller"를 전달했다. 이것은 웹팩이 파일을 처리할 때 청크로 분리하는데 그 청크 이름을 설정한 것이다.

그리고 나서 프라미스를 리턴하는 getController() 함수로 모듈을 가지고 와 사용 했다.

변경한 웹팩 설정 파일도 다시 복구해야 한다. 엔트리 포인트를 main만 남겨두고 optimization 에 설정한 SplitChunksPlugin 옵션도 제거한다.

빌드 하면 자동으로 파일이 분리 되어 있다. 엔트리를 분리하지 않아도 controller와 app의 중복 코드를 vendors~controller 파일로 분리한다. 

다이나믹 임포트로 모듈을 가져오면 단일 엔트리를 유지하면서 코드를 분리 할 수 있다.

### externals

조금만 더 생각해보면 최적화 해 볼 수 있는 부분이 있다. 바로 axios 같은 써드파티 라이브러리다. 패키지로 제공될 때 이미 빌드 과정을 거쳤기 때문에 빌드 프로세스에서 제외하는 것이 좋다. 웹팩 설정 중 externals가 바로 이러한 기능을 제공한다.

```js
module.exports = {
	externals: {
		axios: 'axios'
	}
}
```

externals에 추가하면 웹팩은 코드에서 axios를 사용하더라도 번들에 포함하지 않고 빌드한다. 대신 이를 전역 변수로 접근하도록 하는데 키로 설정한 axios가 그 이름이다.

axios는 이미  node_modules에 위치해 있기 때문에 이를 웹팩 아웃풋 폴더로 옮기고 index.html에서 로딩해야 한다. 

파일을 복사하는 CopyWebpackPlugin을 설치한다.

```shell
npm i -d copy-webpack-plugin
```

플러그인을 사용해 라이브러리를 복사한다.

```js
const CopyPlugin = require('copy-webpack-plugin');

module.exports = {
  plugins: [
    new CopyPlugin([{
      from: './node_modules/axios/dist/axios.min.js',
      to: './axios.min.js' // 아웃풋에 폴더에 들어간다.
    }])
  ]
}
```

마지막으로 index.html에서는 axios를 로딩하는 코드를 추가한다.

```js
<!-- src/index.html -->
  <script type="text/javascript" src="axios.min.js"></script>
</body>
</html>
```

axios 는 직접 추가 했지만 번들링한 결과물은 HtmlWebpackPlugin이 주입해 주는 것을 잊지 말자.

다시 빌드 해 보면 axios는 빌드 하지 않고 복사만 한다. controller와 main 이 분리되어 있다. 이 전에는 공통의 코드인 axios가 vendor~.js로 분리 되었는데 지금은 파일 조차 없다. 만약 써드파티 라이브러리 외에 공통의 코드가 있다면 이 파일로 분리 되었을 것이다.

이렇게 써드파티 라이브러리를  externals로 분리하면 용량이 감소할 뿐만 아니라 빌드 시간도 줄어들고 덩달아 개발 환경도 가벼워 질 수 있다.

# 정리

프론트엔드 개발 환경은 빠르게 변화하고 있다. 기본에 충실에 변화에 빠르게 대응할 수 있도록 하자.