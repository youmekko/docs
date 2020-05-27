# 린트

# ESLint

ESLint가 린트중에 가장 많이 사용하는 것이다.

- 포맷팅 - 일관된 코드 스타일 유지하도록 해서 개발자로 하여금 쉽게 읽히는 코들르 만들어 준다.
- 코드 품질 - 어플리케이션의 잠재적인 오류나 버그를 예방한다. (사용하지 않는 변수 쓰지 않기, 글로벌 스코프 함부로 다루지 않기 등)

```bash
npm install enlint
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
npm install prettier
~~~



