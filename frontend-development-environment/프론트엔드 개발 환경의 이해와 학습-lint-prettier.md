# 린트

# ESLint

ESLint가 린트중에 가장 많이 사용하는 것이다.

- 포맷팅 - 일관된 코드 스타일 유지하도록 해서 개발자로 하여금 쉽게 읽히는 코들르 만들어 준다.
- 코드 품질 - 어플리케이션의 잠재적인 오류나 버그를 예방한다. (사용하지 않는 변수 쓰지 않기, 글로벌 스코프 함부로 다루지 않기 등)

```bash
npm i enlint
```

### Rules

ESLint는 검사 규칙을 미리 정해 놓았는데 이것을 Rules라고 한다. ESLint 공식 문서에 Rules 메뉴에서 목록을 확인 할 수 있다. ([eslint.org/docs/rules](https://eslint.org/docs/rules/))

(ESLint를 실행하기 위해서는 config파일이 있어야 한다. )

```javascript
//.enslintrc.js
module.exports = {
	rules: {
		'no-unexpected-multiline' : 'error'
	}
}
```

### Extensible Config

이러한 규칙을 여러개 미리 정해 놓은 것이 eslint:recommended 설정이다. 규칙 목록 중에 왼쪽에 체크 표시 되어 있는 것이 이 설정에서 활성화 되어 있는 규칙이다.

```javascript
//.enslintrc.js
module.exports = {
	extends: [
		'eslint:recommended'
	]
}
```

이 설정 외에 규칙이 필요하다면 rules 속성에 추가해서 확장 할 수 있따.

ESLint에는 기본으로 제공하는 설정 외에 자주 사용하는 두가지가 있다.

- airbnb - eslint-config-airbnb-base 패키지로 제공된다.
- standard - eslint-config-standart 패키지로 제공된다.

### 초기화

사실 이런 설정은 `--init` 옵션을 추가하면 손쉽게 구성할 수 있다.

```bash
npx eslint --init
```

### ESLint 실습 (정리)

(1) eslint 설치

```bash
npm i eslint
```

(2) /.enslintrc.js 구성

```bash
npx eslint --init
```

(3) package.json 수정

```javascript
scripts: {
	"lint": "eslint src --fix"
}
```

(4)

```bash
npm run lint
```



# 프리티어

ESLint 보다 코드를 더 예쁘게 만든다.

ESLint는 포맷팅과 코드 품질을 담당한다. 이 중에 포맷팅을 기능을 강화한 것이 프리티어다. 반면에 코드 품질에 관한 기능은 없다.

~~~bash
npm i prettier
~~~

~~~bash
npx prettier app.js ---write //옵션
~~~



[before] 

~~~javascript
console.log()
//또는 console.log();;;;;;;;;
~~~

[after] 

~~~bash
console.log();
~~~



프리티어의 강점은 ESLint가 고칠 수 없는 코드도 자동으로 고쳐 준다는 것이다.  (ESLint는 리포팅만 해준다.)

[before]

~~~javascript
console.log('--------------- 매 우 긴 문 장 입 니 다 8 0 자 가 넘 는 코 드 입 니 다 . ---------------');
~~~

[after]

~~~javascript
console.log(
'--------------- 매 우 긴 문 장 입 니 다 8 0 자 가 넘 는 코 드 입 니 다 . ---------------'
);
~~~

 

[before]

~~~javascript
foo(reallyLongArg(), omgSoManyParameters(), IShouldRecatorThis(), isThereSeriouslyAnotherOne());
~~~

[after]

~~~javascript
foo(
	reallyLongArg(), 
	omgSoManyParameters(), 
	IShouldRecatorThis(), 
	isThereSeriouslyAnotherOne()
);
~~~



여전히 ESLint를 사용해야 하는 이유는 남아있다. 포맷팅은 프리티어에 맡기더라도 코드 품질과 관련된 검사는 ESLint의 몫이기 때문이다. 따라서 이 둘을 같이 사용하는 것이 최선이다.

프리티어는 이러한 ESLint와 통합 방법을 제공한다. eslint-config-prettier는 프리티어와 충동하는 ESLint 규칙을 끄는 역할을 한다.



### ESLint + Prettier 통합 방법

~~~bash
npm i -D eslint-config-prettier
~~~

`.eslintrc.js`

~~~javascript
"extends" : ["exlint:recommended", "eslint-config-prettier"]
~~~



하지만 이렇게 두 번 하면 불편하니까...

~~~bash
npx eslint app.js --fix
~~~

~~~bash
npx prettier app.js --write
~~~



추가로 eslint-plugin-prettier를 설치한다. 프리티어 규칙을 ESLint 규칙으로 추가하는 플러그 인이다. 프리티어의 모든 규칙이  ESLint로 들어오기 때문에 ESLint만 실행하면 된다.

~~~bash
npm i -D eslint-plugin-prettier
~~~

`.eslintrc.js`

~~~javascript
"plugins": [
  "prettier"
],
"rules": {
  "prettier/pretiter": "error"
}
~~~

~~~javascript
npx eslint app.js ---fix
~~~



[before]

~~~javascript
var foo = '';

console.log();;;;;
~~~

[after]

~~~javascript
var foo = ''; //사용하지 않는 변수는 리포팅만

console.log();
~~~



이렇게 셋팅해도 되고 혹은 `.eslintrc.js`를 아래처럼 수정해 줘도 된다.

~~~javascript
{
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ]
}
~~~



# 자동화

린트는 코딩할 때마다 수시로 실행해야 하는데 이러한 일은 자동화 하는 것이 좋다. '깃 훅을 사용하는 방법' 과 '에디터 확장 도구를 사용하는 방법' 을 각각 소개한다.

### 변경한 내용만 검사

소스 트래킹 도구로 깃을 사용한다면 깃 훅을 이용하는 것이 좋다. 커밋 전, 푸시 전 등 커맨드 실행 시점에 끼여들 수 있는 훅을 제공한다.

**husky** 는 깃 훅을 쉽게 사용할 수 있는 도구이다. 

~~~bash
npm i -D husky
~~~



package.json

~~~javascript
{
  "husky": {
    "hooks": {
      "pre-commit": "echo \"이것은 커밋 전에 출력 됨\"" //"eslint app.js --fix"
    }
  }
}
~~~

이렇게 하면 커밋 직전에 명령어를 실행 할 수 있다.



코드가 많아지면 ESLint 실행이 느려질 수 있다. 최적화 할 수 있는 방법은 커밋을 할 때 커밋한 파일만 린트를 돌리는 것이다. 해당 기능을 해주는 것이 lint-staged 패키지 이다.

~~~bash
npm i -D lint-staged
~~~



package.json

~~~javascript
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
},
{
  "lint-staged": {
    "*.js": "eslint --fix"
  }
}
~~~

이렇게 하면 변경된 파일만 린트를 수행하고 린트를 통과해야만 커밋을 할 수 있다. 통과하지 못하면 커밋에 실패한다.



### 에디터에서 검사 할 수 있는 방법

* vscode에서 플러그인을 설치해서 쓸 수 있다.

  - eslint

* 저장할 때 자동으로 검사할 수도 있다. 에디터 설정 - 작업 영역 - settings.json

  ~~~javascript
  {
    "edutir.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
  }
  ~~~

  