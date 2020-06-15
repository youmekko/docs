# 트렐로 개발을 통한  Vuejs, Vues, Vue-Router 실전 기술

# 시작하기

## 1강 강의소개

### 개발순서

* 요구사항 정리
* 기술 조사
* 기능 구현

#### 사용 하는 것들

* 상태 관리를 위한 vuex
* 라우팅을 위한  vue-router
* api  통신을 위한 axios
* drag and drop을 위한 dragular 

## 2강 요구사항 분석1

## 3강 요구사항 분석

* 인증 : 로그인, 로그아웃

* 보드작업

  |        | 생성               | 조회               | 수정                                   | 이동               | 삭제               |
  | ------ | ------------------ | ------------------ | -------------------------------------- | ------------------ | ------------------ |
  | 보드   | <strong>O</strong> | <strong>O</strong> | <strong>O</strong><br />(타이틀,색상)  | <strong>X</strong> | <strong>O</strong> |
  | 리스트 | <strong>O</strong> | <strong>X</strong> | <strong>O</strong><br />(타이틀)       | <strong>O</strong> | <strong>O</strong> |
  | 카드   | <strong>O</strong> | <strong>O</strong> | <strong>O</strong><br />(타이틀, 설명) | <strong>O</strong> | <strong>O</strong> |

* 기본 플로우

  * 홈페이지 접속 (비인가 요청은 로그인 페이지 이동)
  * 로그인 페이지 접속 : 로그인 수행
  * 홈페이지 리다이렉트
  * 보드 목록 조회
  * 보드 생성 : 생성한 보드 화면 이동
  * 보드 조회 : 자동 생성된 리스트 나열 (Todo, Doing, Done)
  * 카드 생성 : 타이틀 입력
  * 카드 상세 조회 : 모달창
  * 카드 수정 : 타이틀, 설명
  * 카드 이동1 : 리스트 내에서 이동
  * 카드 이동2 : 리스트 간 이동
  * 카드 삭제
  * 보드 세팅 : 사이드바
  * 보드 삭제 : 삭제 후 홈페이지 이동

* 추가 기능

  * 보드 수정: 색상 변경

  * 보드 수정 : 타이틀 변경
  * 리스트 생성
  * 리스트 수정 : 타이틀 변경
  * 리스트 이동
  * 리스트 삭제

## 4강 코드   스캐폴딩

* vue-cli 설치 `npm install -g vue-cli`
* 프로젝트 폴더 생성 
* `vue init webpack-simple`
* `npm i(install)`
* 개발서버 구동 `npm run dev` 

## 5강 라우터의 필요성

웹 서비스는 여러개의 웹 페이지로 이루어져 있다. 각 페이지는 주소에 따라서 식별되는데 이것을 **라우팅**이라고 한다. 라우팅 하는 주체는 서버가 될수도 있고 브라우저가 될 수도 있다. 서버가 라우팅 하는 것을 서버 라우팅이라 하고 브라우저가 라우팅 하는 것을 브라우저 라우팅이라고 한다.

### 서버라우팅 

* 매번 주소를 요청할 때 마다 화면이 갱신된다.
* 네이버, 구글 등

### 브라우저 라우팅

* 주소를 매번 요청하더라도 화면이 갱신되지 않는다.
* 대신, 화면에 필요한 데이터만 서버에 따로 요청해서 화면을 갱신하는 방법을 사용한다.
* 서버 라우팅과 비교해서 좀 더 효율적으로 화면을 갱신한다.
* 구글 메일, 트렐로 등
* 브라우저 라우팅은 주소 요청시 브라우저에서 처리하고 데이터는 백엔드 API를 통해서 서버로 부터 받아온 뒤에 마지막으로 그 데이터를 이용해 화면에 뿌리는 구조이다.
* 브라우저에서 동작하는 Vue.js는 이러한 라우팅 기능도 수행할 수 있다.

## 6강 라우터 직접 만들기

메인 페이지인 Root경로와 로그인을 위한 로그인 경로 라우팅 로직을 먼저 만들어보자.

현재 Vue 어플리케이션이 동작할 때 제일 처음 동작하는 부분은 main.js이다.

Webpack을 통해서 어플리케이션들이 번들링 되는데 entry 포인트가 src/main.js로 설정 되어 있다.

그래서 main.js에 라우팅 로직을 만들면 된다.

루트 경로로 접속 했을 때 app 컴포넌트가 라우팅 되는 것이다.

~~~javascript
import Vue from 'vue'
import App from './App.vue'

const Login = { template: '<div>Login Page</div>' }

const routes  = {
  '/' : App,
  '/login' : Login
}

new Vue({
  el: '#app',
  computed: {
    VueComponent(){
      return routes[window.location.pathname] ||
        { template : '<div>Page not found</div>'}
    }
  },
  render(h){
    return h(this.VueComponent)
  }
})

~~~

이렇게 만든 라우팅 기능을 미리 구현해 둔것이 vue-router라는 라이브러리이다. vue 진영에서 가장 많이 사용하는 라이브러리이다.

## 7강 vue-router

vur-router 설치 `npm i vue-router --save`

우리는 서버 라우팅이 아니라 브라우저 라우팅을 구현하려고 하고있다. 브라우저에서 라우팅 하는 것을 Single Page App이라고 하고 이것을 구현하기 위해서는 vue-router를 사용 해야 하는 것이다. 

> Vue.js를 사용한다면 이미 컴포넌트로 앱을 구성하고 있을 것입니다. Vue 라우터를 함께 사용할 때 추가로 해야하는 것은 라우트에 컴포넌트를 매핑한 후, 어떤 주소에서 렌더링할 지 알려주는 것 뿐입니다.
> https://router.vuejs.org/kr/guide/

~~~javascript
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'

//미들웨어
Vue.use(VueRouter)

const Login = { template: '<div>Login Page</div>' }
const NotFound = { template: '<div>Page not found</div>' }


const router = new VueRouter({
  routes: [
    {path : '/', component : App},
    {path : '/login', component : Login },
    {path : '*', component: NotFound}
  ]
})


new Vue({
  el: '#app',
  router,
  render: h => h({template: '<router-view></router-view>'})
})

~~~

특이하게 #가 붙는데, 브라우저에서 라우팅 할때 해쉬뱅 (hashbang) 모드라는게 동작하고 있기 때문이다. 브라우저 히스토리 API가 없을 때 해시뱅 모드를 사용하지만 크롬의 경우는 히스토리 API가 있기 때문에 해시뱅 모드가 아닌 히스토리 모드를 사용하면 된다.

~~~javascript
const router = new VueRouter({
  mode: 'history',
  routes: [
    {path : '/', component : App},
    {path : '/login', component : Login },
    {path : '*', component: NotFound}
  ]
})
~~~

## 8강 라우터 인스턴스

main.js 안에 라우팅 로직이 많이 들어있으므로 라우팅 로직만 따로 분리해 보자.
src 디렉토리 아래 router 디렉토리를 만들고 index.js 파일을 만든다.
(index.js 말고도 더 많은 서브 라우터를 만들 수 있다.)

router/index.js

~~~javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import App from '../App.vue'

//미들웨어
Vue.use(VueRouter)

const Login = {template: '<div>Login Page</div>'}
const NotFound = {template: '<div>Page not found</div>'}


const router = new VueRouter({
  /*
  브라우저에서 라우팅 할때는 해쉬뱅(Hashbang)모드라는게 동작하는데 (브라우저 히스토리 API가 없을 때 사용)
  크롬의 경우는 history API가 있기 때문에 해시뱅 모드가 아닌 히스토리 모드를 사용하면 된다.
  */
  mode: 'history',
  routes: [
    {path: '/', component: App},
    {path: '/login', component: Login},
    {path: '*', component: NotFound}
  ]
})

export default router

~~~

main.js

~~~javascript
import Vue from 'vue'
import router from './router'


new Vue({
  el: '#app',
  router,
  render: h => h({template: '<router-view></router-view>'})
})

~~~

## 9강 라우터 뷰

main.js의 `<router-view></router-view>` 부분이 요청 URL에 따라서 화면이 바뀌는 부분이다.  그런데 이 프로젝트에서는 App.vue 라는 것이 루트 컴포넌트 역할을 한다. 그래서 이 루트 컴포넌트를 기준으로 URL에 따라서 컴포넌트를 바꿔치기 해야한다. 그래서 main.js에 있는 <router-view></router-view>를 App.vue로 옮겨보자.

App.vue

~~~javascript
<template>
  <div id="app">
    여기서부터 코드를 시작합니다.
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    }
  }
}
</script>

<style>
</style>

~~~

router/index.js

~~~javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../components/Home.vue'
import Login from '../components/Login.vue'
import NotFound from '../components/NotFound.vue'

//미들웨어
Vue.use(VueRouter)

const router = new VueRouter({
  /*
  브라우저에서 라우팅 할때는 해쉬뱅(Hashbang)모드라는게 동작하는데 (브라우저 히스토리 API가 없을 때 사용)
  크롬의 경우는 history API가 있기 때문에 해시뱅 모드가 아닌 히스토리 모드를 사용하면 된다.
  */
  mode: 'history',
  routes: [
    {path: '/', component: Home},
    {path: '/login', component: Login},
    {path: '*', component: NotFound}
  ]
})

export default router
~~~

main.js

~~~javascript
import Vue from 'vue'
import router from './router'
import App from './App.vue'

new Vue({
  el: '#app',
  router,
  //entry po
  render: h => h(App)
})

~~~

src/components 들에 각  URL 별로 보여질 컴포넌트들도 만든다. (Home/Login/NotFound)

## 10강 라우터 링크

`<router-link>`는 라우터 지원 앱에서 사용자 내비게이션을 가능하게하는 컴포넌트이다. (뷰 라우터가 제공하는) 내비게이션을 위한 컴포넌트이다.

<router-link>는 다음과 같은 이유로 하드 코드된 `<a href="...">`보다 선호된다.

* HTML5 히스토리 모드와 해시 모드에서 모두 동일한 방식으로 작동하므로 모드를 트랜지션하기로 결정하거나 라우터가 IE9에서 해쉬모드로 트랜지션 한 경우 변경할 필요가 없다.
* HTML5 히스토리 모드에서 <router-link>는 클릭 이벤트를 차단해 부라우저가 페이지를 다시 로드하지 않도록 한다.
* ~~HTML5 히스토리 모드에서 base옵션을 사용할 때 `to` prop의 URL에 이를 포함 할 필요가 없다.~~

components/NavBar.vue  컴포넌트 추가

~~~javascript
<template>
    <div>
         <!--
          히스토리 모드를 쓸 때는  <a href="/">Home</a>
          해시뱅 모드르 사용할 때는 <a href="/#/> 이처럼 라우팅 해야한다.
          <router-link>를 쓰는 장점!
          -->
      <router-link to="/">Home</router-link>
      <router-link to="/login">Login</router-link>
    </div>
</template>

<script>
    export default {
    }
</script>

<style scoped>

</style>

~~~

App.vue

~~~javascript
<template>
  <div id="app">
    <navbar />
    <router-view></router-view>
  </div>
</template>

<script>
import navbar from './components/NavBar.vue'

export default {
  name: 'app',
  components : { navbar },
  data () {
    return {
    }
  }
}
</script>

<style>
</style>

~~~

## 11강 동적 라우트 매칭

우리가 router를 설정할 때 Vue.use라는 함수를 통해서 VueRouter를 미들웨어로 추가했다. 그렇기  때문에 Vue를 통해서 만든 vue 인스턴스는 $route라는 변수를 통해서 라우터 정보에 접근할 수 있다. 

index.js

~~~javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../components/Home.vue'
import Login from '../components/Login.vue'
import NotFound from '../components/NotFound.vue'
import Board from '../components/Board.vue'

//미들웨어
Vue.use(VueRouter)

const router = new VueRouter({
  /*
  브라우저에서 라우팅 할때는 해쉬뱅(Hashbang)모드라는게 동작하는데 (브라우저 히스토리 API가 없을 때 사용)
  크롬의 경우는 history API가 있기 때문에 해시뱅 모드가 아닌 히스토리 모드를 사용하면 된다.
  */
  mode: 'history',
  routes: [
    {path: '/', component: Home},
    {path: '/login', component: Login},
    {path: '/b/:bid', component: Board},
    {path: '*', component: NotFound}
  ]
})

export default router

~~~

Board.vue

~~~javascript
<template>
    <div>
      Board
      <div>bid: {{ bid }}</div>
    </div>
</template>

<script>
    export default {

      data(){
        return {
            bid: 0
        }
      },

      //Board가 생성될 때 실행되는 훅이 Create
      created(){
        //$route 객체 로그로 찍어보자
        console.log("@@@route", this.$route);
        console.log("@@@bid", this.$route.params.bid);

        //로그에서 확인할 수 있는 것처럼 this.$route 객체를 통해서 라우팅 정보를 받아낼 수 있다.
        this.bid = this.$route.params.bid;
      }
    }
</script>

<style scoped>

</style>

~~~



## 12강 중첩 라우트

Card.vue

~~~javascript
<template>
  <div>
    Card
    <div>cid: {{cid}}</div>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        cid: 0
      }
    },
    watch:{
      '$route'() {
        this.cid = this.$route.params.cid;
      }
    },
    created(){
      this.cid = this.$route.params.cid;
    }
  }
</script>

<style scoped>

</style>

~~~

index.js

~~~javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../components/Home.vue'
import Login from '../components/Login.vue'
import NotFound from '../components/NotFound.vue'
import Board from '../components/Board.vue'
import Card from '../components/Card.vue'

//미들웨어
Vue.use(VueRouter)

const router = new VueRouter({
  /*
  브라우저에서 라우팅 할때는 해쉬뱅(Hashbang)모드라는게 동작하는데 (브라우저 히스토리 API가 없을 때 사용)
  크롬의 경우는 history API가 있기 때문에 해시뱅 모드가 아닌 히스토리 모드를 사용하면 된다.
  */
  mode: 'history',
  routes: [
    {path: '/', component: Home},
    {path: '/login', component: Login},
    {path: '/b/:bid', component: Board, children:[
        {path: '/c/:cid', component: Card}
      ]},
    {path: '*', component: NotFound}
  ]
})

export default router

~~~



중첩 라우터를 사용하는 이유는 보드에서 카드를 선택해서 들어갈때 보드는 백그라운드에 깔려있고 카드 내용은 보드 위에 팝업으로 띄워졌기 때문이다.

Board.vue

~~~javascript
<template>
    <div>
      Board
      <div>bid: {{ bid }}</div>
      <router-link :to="`/b/${bid}/c/1`">Card1</router-link>
      <router-link :to="`/b/${bid}/c/2`">Card2</router-link>
      <hr/>
      <router-view></router-view>
    </div>
</template>

<script>
    export default {

      data(){
        return {
            bid: 0
        }
      },

      //Board가 생성될 때 실행되는 훅이 Create
      created(){
        //$route 객체 로그로 찍어보자
        console.log("@@@route", this.$route);
        console.log("@@@bid", this.$route.params.bid);

        //로그에서 확인할 수 있는 것처럼 this.$route 객체를 통해서 라우팅 정보를 받아낼 수 있다.
        this.bid = this.$route.params.bid;
      }
    }
</script>

<style scoped>

</style>

~~~



## 13강 데이터 불러오기

vue router들을 통해서 백엔드 API 연동시 어떻게 화면을 만들 수 있는지 살펴보자

Board.vue

~~~javascript
<template>
  <div>
    Board

    <div v-if="loading">loading board...</div>
    <div v-else>
      <div>bid: {{ bid }}</div>
      <router-link :to="`/b/${bid}/c/1`">Card1</router-link>
      <router-link :to="`/b/${bid}/c/2`">Card2</router-link>
    </div>
    <hr/>
    <router-view></router-view>
  </div>
</template>

<script>
  export default {

    data(){
      return {
        bid: 0,
        loading: false
      }
    },

    //Board가 생성될 때 실행되는 훅이 Create
    created(){

      this.fetch();

      //$route 객체 로그로 찍어보자
      console.log("@@@route", this.$route);
      console.log("@@@bid", this.$route.params.bid);
    },

    methods: {
      //백엔드 API 호출 데이터 요청하는 메서드
      fetch(){
        this.loading = true;

        //백엔드 데이터 로딩 시뮬레이션
        setTimeout(()=>{
          //로그에서 확인할 수 있는 것처럼 this.$route 객체를 통해서 라우팅 정보를 받아낼 수 있다.
          this.bid = this.$route.params.bid;
          this.loading = false;
        }, 500)


      }
    }
  }
</script>

<style scoped>

</style>

~~~

Card.vue

~~~javascript
<template>
  <div>
    Card
    <div v-if="loading">loading card...</div>
    <div v-else>
      <div>cid: {{cid}}</div>
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        cid: 0,
        loading: false
      }
    },
    /***/
    watch:{
      '$route': {
        handler : 'fetchData',
        immediate: true
      }
    },
    methods: {
      fetchData(){
        this.loading = true;
        setTimeout(()=>{
          this.cid = this.$route.params.cid;
          this.loading = false;
        },500)

      }
    }
  }
</script>

<style scoped>

</style>

~~~

라우터 변경에 따라 어떻게 데이터를 호출할 수 있는지 알아 봤다.



# API 통신

## 14강 백엔드 API 살펴보기

spa에서는 데이터를 백엔드 서버로부터 가지고 온다. 서버의 역할은 데이터 베이스에 있는 데이터를 클라이언트에게 제공하는 것이다. htpp 프로토콜을 이용한 api형태로 데이터를 제공한다.

이 예제에서는 `http://github.com/jeonghwan-kim/lecture-vue-trello-sever.git` 을 clone해 진행한다.

해당 repository를 clone을 npm intstall하고 `npm run dev` 로 서버를 구동하면 API Document를 확인할 수 있다. 

`curl localhost:3000/health` 등 명령어를 그대로 복사해 터미널에 입력하면 데이터를 얻어 올수 있고 -v 옵션을 줄 시 더 디테일한 정보를 얻어 올 수 있는데 화살표가 오른쪽으로 `>` 되어있는 것은 request 정보고 왼쪽 `<` 으로 되어 있는 것은 response 정보다.

_추가로 homebrew를 통해 jq(Litteweight and flexible command-line JSON processor)도 설치해보자_



## 15강 Ajax - XMLHttpRequest 객체

이전 강의에서는 api를 호출할 때 터미널 도구인 curl을 이용해 데이터를 요청했다.  그런데 trello 서비스에서 api를 쓰려면, 즉 브라우저단에서 api를 호출하려면 Javascript를 이용해 호출해야한다.

Javascript에는 XMLHttpRequeset 라는 객체가 있는데 이것이 바로 http 호출을 만들때 사용하는 객체이다. 이것을 이용해 Api를 호출해보자.

Home.vue

~~~javascript
<template>
  <div>
    <div>Home</div>
    <div>
      Board List :
      <div v-if="loading">Loading....</div>
      <div v-else>
        API result : {{apiRes}}
      </div>
      <ul>
        <li>
          <router-link to="/b/1">Board 1</router-link>
        </li>
        <li>
          <router-link to="/b/2">Board 2</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        loading: false,
        apiRes: ''
      }
    },
    created() {
      this.fetchData();
    },
    methods: {
      fetchData() {
        this.loading = true;

        const req = new XMLHttpRequest();

        //@Param (method, address)
        req.open('GET', 'http://localhost:3000/health')

        //클라이언트에서 백엔드 서버로 요청을 날라간다.
        req.send();

        //요청이 완료되면 load 라는 이벤트가 발생한다.
        req.addEventListener('load', () => {
          this.loading = false;
          this.apiRes = {
            status: req.status,
            statusText: req.statusText,
            response: JSON.parse(req.response)
          }
        })
      }
    }
  }
</script>

<style scoped>

</style>
~~~

요청이 잘 갔는지 확인하려면 chrom 개발자 도구에서 Network 탭을 열고  XHR (XMLHttpRequest)로 필터를 걸고 보면 된다.

이번 강의에서는 순수 브라우저 Api 만 이용해서  http콜을 날려봤다. 보통은 브라우저별로 XHR객체가 지원되지 않을 수도 있다. 그래서 보통 Jquery의 ajax 함수 등을 사용한다. 라이브러리를 사용하면 조금 더 단순하게 http 콜을 만들 수 있다는 장점이 있다. 다음 강의에서는 vuejs에서 가장 많이 사용하는 axios라는 http라이브러리를 소개 한다.



## 16강 Axios 

Axios는 Promise 기반의 Http 클라이언트다. 브라우저에서도 쓸 수 있고 노드에서도 쓸 수 있다.

* XMLHttpRequest 객체를 만들어준다.
* Nodejs에서 Http 리퀘스트를 만들어준다.
* 프로미스 기반이라 간결하게 비동기 로직을 처리할 수 있다.
* 요청과 응답사이에 어떤 로직을 넣을 수 있다(Intercept).
* 요청하거나 응답 받을 때 데이터를 전달하는 역할을 한다. 
* 요청을 취소할 수도 있다.
* 자동으로 JSON 데이터를 변환해준다.

대부분의 브라우저에서 지원한다.

설치 `npm install axios`



## 17강 보드 조회 API 연동

home.vue

~~~javascript
<template>
  <div>
    <div>Home</div>
    <div>
      Board List :
      <div v-if="loading">Loading....</div>
      <div v-else>
        <div v-for="board in boards" :key="board.id">
          {{board}}
        </div>
      </div>
      <ul>
        <li>
          <router-link to="/b/1">Board 1</router-link>
        </li>
        <li>
          <router-link to="/b/2">Board 2</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        loading: false,
        boards: []
      }
    },
    created() {
      this.fetchData();
    },
    methods: {
      fetchData() {
        this.loading = true;
        axios.get('http://localhost:3000/boards').then((res) => {
          this.boards = res.data;
        }).catch(() => {
          this.$router.replace('/login');
        }).finally(() => {
          this.loading = false;
        })
      }
    }
  }
</script>

<style scoped>

</style>

~~~



## 18강 Axios 실전에서 사용하기

aixos 같은 서드파티 라이브러를 사용할 때 하나의 팁은 라이브러리를 랩핑해서 사용하는 것이다. 위에서처럼 직접 axios를 가져다 사용하기 시작하면 코드가 많아질수록 api 요청이 필요한 부분마다 직접 라이브러를 호출하게 된다. 그렇게 되면 라이브러리에 강하게 의존하는 코드를 만들게 된다. 상황이 바뀌어서 라이브러리를 바꿀 때는 하드코딩 된 코드들을 모두 변경해야 된다. 따라서 의존적인 코드를 최소화 하기 위해서 Axios를 호출하는 모듈을 별도로 만들어주는 것이 방법이다. 

/api/index.js

~~~javascript
import axios from 'axios'
import router from '../router'

const DOMAIN = 'http://localhost:3000'
const UNAUTHORIZED = 401

const onUnauthorized = () => {
  router.push('/login')
}

const request = (method, url, data) => {
  return axios({
    method,
    url: DOMAIN + url,
    data
  }).then((result) => {
    result.data;
  }).catch((result) => {
    const {status} = result.response;
    if (status === UNAUTHORIZED) {
      return onUnauthorized();
    }
    throw Error(result)
  })
}

export const board = {
  fetch(){
    return request('get', '/boards')
  }
}

~~~

Home.vue

~~~javascript
<template>
  <div>
    <div>Home</div>
    <div>
      Board List :
      <div v-if="loading">Loading....</div>
      <div v-else>
        <div v-for="board in boards" :key="board.id">
          {{board}}
        </div>
      </div>
      <ul>
        <li>
          <router-link to="/b/1">Board 1</router-link>
        </li>
        <li>
          <router-link to="/b/2">Board 2</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

  import {board} from '../api'

  export default {
    data() {
      return {
        loading: false,
        boards: []
      }
    },
    created() {
      this.fetchData();
    },
    methods: {
      fetchData() {
        board.fetch().then(data => {
          this.boards = data;
        }).finally(_=>{
          this.loading = false;
        })
      }
    }
  }
</script>

<style scoped>

</style>

~~~

위처럼 컴포넌트에서 axios를 호출하는 것이 아니라 api디렉토리에 있는 aixos모듈들을 사용한다.



## 19강 인증 API

인증 API를 어떻게 사용해야 하는지 알아보자.

boardAPI를 호출할 때 토큰 정보를 추가 하게 되면... 

대부분의 API 에서 사용되는 토큰 정보는 클라이언트 즉 브라우저 쪽에서 저장이 필요하다. 이 프로젝트에서는 localStorage에다 저장한다.



## 20강 네비게이션 가드

메인 페이지에서는 getBoards  API 결과를 확인한 뒤에 401이 뜨면 로그인 페이지로 이동 한다. 다른 페이지에서도 401응답을 받으면 로그인 페이지로 이동한다.

API를 호출하기 전에 로그인 여부를 확인할 수 있는 방법을 없을까?

인증을 완료하고 난 토큰 정보를 localStorage라는 브라우저 저장소에 저장하기로 한다. 그렇다면 로컬 스토리지에 있는 토큰의 유무를 통해 로그인 여부를 확인할 수 있다. 그래서 이 정보가 있으면 로그인 한것으로 간주하고 아니면 로그인 페이지로 이동하도록 변경하면 된다. 메인 페이지뿐 아니라 모든 페이지에 적용한다. 

따라서 페이지로 라우팅 하기 직전에 토큰을 확인하고 있으면 라우팅 동작을 하고 없으면 로그인 페이지로 리다이렉트 하는 기능을 만들어보자.

여기서 뷰라우터의 기능을 사용할 수 있는데 네이게이션 가드라는 기능이다. 이것은 라우팅 직전에 어떤 로직을 추가할 수 있는 기능이다.

네비게이션 가드는 뷰 라우터의 beforeEnter라는 함수를 사용하면 된다.

router/index.js

~~~javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../components/Home.vue'
import Login from '../components/Login.vue'
import NotFound from '../components/NotFound.vue'
import Board from '../components/Board.vue'
import Card from '../components/Card.vue'

//미들웨어
Vue.use(VueRouter)

const requireAuth = (to, from, next) => {
  const isAuth = localStorage.getItem('token');
  const loginPath = `/login?rPath=${encodeURIComponent(to.path)}`;
  isAuth ? next() : next(loginPath);
}

const router = new VueRouter({
  /*
  브라우저에서 라우팅 할때는 해쉬뱅(Hashbang)모드라는게 동작하는데 (브라우저 히스토리 API가 없을 때 사용)
  크롬의 경우는 history API가 있기 때문에 해시뱅 모드가 아닌 히스토리 모드를 사용하면 된다.
  */
  mode: 'history',
  routes: [
    {path: '/', component: Home, beforeEnter: requireAuth},
    {path: '/login', component: Login},
    {path: '/b/:bid', component: Board, beforeEnter: requireAuth, children:[
        {path: 'c/:cid', beforeEnter: requireAuth, component: Card}
      ]},
    {path: '*', component: NotFound}
  ]
})

export default router

~~~



### 21강 로그인

네이게이션 가드를 통해서 인증되지 않는 요청은 로그인 페이지까지 이동을 시켰다. 이제 본격적으로 로그인 페이지를 만들어보자.

일단 App.vue에 아래의 css를 추가한다.

~~~css
html, body, #app {
    height: 100%;
    margin: 0;
  }

  #app {
    display: flex;
    flex-direction: column;
  }

  .container {
    flex-grow: 1;
    position: relative;
  }

  .btn {
    border-radius: 3px;
    padding: 6px 8px;
    background-color: #e2e4e6;
    border: none;
    display: inline-block;
    color: #fff;
    font-size: 14px;
    line-height: 20px;
    font-weight: 700;
    cursor: pointer;
  }

  .btn-success {
    background-color: #5aac44;
    box-shadow: 0 1px 0 #519839;
  }

  .form-control {
    width: 100%;
    box-sizing: border-box;
    background-color: #e2e4e6;
    border: 1px solid #cdd2d4;
    border-radius: 3px;
    display: block;
    margin-bottom: 12px;
    padding: 6px 8px;
    transition: background-color .3s;
  }

  input[type=text].form-control,
  input[type=passwod].form-control,
  textarea.form-control {
    font-size: 14px
  }

  form-control:focus {
    background-color: #fff;
  }

~~~

다음은 login.vue파일을 수정해보자.

~~~javascript
<!--suppress ALL -->
<template>
  <div class="login">
    <h2>Login to Trello</h2>
    <form @submit.prevent="onSubmit">
      <div>
        <label for="email">Email</label>
        <input class="form-control" type="text" name="email"
               v-model="email" autofocus placeholder="e.g., text@test.com">
      </div>
      <div>
        <label for="password">Password</label>
        <input class="form-control" type="text" name="passowrd"
               v-model="password" placeholder="123123">
      </div>
      <button class="btn" :class="{'btn-success' : !invalidForm}" type="submit"
      :disabled="invalidForm">Login</button>
    </form>
    <p class="erorr" v-if="error">{{error}}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        email: '',
        password: '',
        error: ''
      }
    },
    computed: {
      invalidForm() {
        return !this.email || !this.password
      }
    },
    methods: {
      onSubmit() {
        console.log(this.email, this.password)
      }
    }
  }


</script>

<style scoped>
  .login {
    width: 400px;
    margin: 0 auto;
  }

  .error {
    color: #f00;
  }
</style>

~~~

이제 남은 것은 로그인 API를 연동해보는 것이다.

api/index.js

~~~javascript
import axios from 'axios'
import router from '../router'

const DOMAIN = 'http://localhost:3000'
const UNAUTHORIZED = 401
const onUnauthorized = () => {
  router.push(`/login?rPath=${encodeURIComponent(location.pathname)}`)
}

const request = (method, url, data) => {
  return axios({
    method,
    url: DOMAIN + url,
    data
  }).then(result => result.data)
    .catch(result => {
      const {status} = result.response
      if (status === UNAUTHORIZED) onUnauthorized()
      throw result.response
    })
}

//토큰 정보를 받아서 axios안에 기능을 추가한다.
//모든 리퀘스트를 날리기 전에 헤더 값을 토큰 정보로 설정하는 역할
export const setAuthInHeader = token => {
  axios.defaults.headers.common['Authorization'] = token ? `Bearer ${token}` : null;
}

const {token} = localStorage
if (token) setAuthInHeader(token)

export const board = {
  fetch() {
    return request('get', '/boards')
  }
}
export const auth = {
  login(email, password) {
    return request('post', '/login', {email, password})
  }
}

~~~

Login.vue

~~~javascript
<template>
  <div class="login">
    <h2>Log in to Trello</h2>
    <form @submit.prevent="onSubmit">
      <div>
        <label for="email">Email</label>
        <input class="form-control" type="text" name="email"
               v-model="email" autofocus placeholder="e.g., test@test.com" />
      </div>
      <div>
        <label for="password">Passwrod</label>
        <input class="form-control" type="password"
               v-model="password" placeholder="123123" />
      </div>
      <button  class="btn" :class="{'btn-success': !invalidForm}" type="submit"
               :disabled="invalidForm">Log In</button>
    </form>
    <p class="error" v-if="error">{{error}}</p>
  </div>
</template>

<script>
  import {auth, setAuthInHeader} from '../api'
  export default {
    data() {
      return {
        email: '',
        password: '',
        error: '',
        rPath: ''
      }
    },
    computed: {
      invalidForm() {
        return !this.email || !this.password
      }
    },
    created() {
      this.rPath = this.$route.query.rPath || '/'
    },
    methods: {
      onSubmit() {
        auth.login(this.email, this.password)
          .then(data => {
            localStorage.setItem('token', data.accessToken)
            setAuthInHeader(data.accessToken)
            this.$router.push(this.rPath)
          })
          .catch(err => {
            this.error = err.data.error
          })
      }
    }
  }
</script>

<style>
  .login {
    width: 400px;
    margin: 0 auto;
  }
  .error {
    color: #f00;
  }
</style>
~~~



### 22강 로그아웃

Navbar.vue

~~~javascript
<template>
    <!--
   히스토리 모드를 쓸 때는  <a href="/">Home</a>
   해시뱅 모드르 사용할 때는 <a href="/#/> 이처럼 라우팅 해야한다.
   <router-link>를 쓰는 장점!
   -->
  <nav class="header">
        <div class="header-logo">
          <router-link to="/">Home</router-link>
        </div>
        <div class="header-auth">
          <a href="" v-if="isAuth" @click.prevent="logout">Logout</a>
          <router-link v-else to="/login">Login</router-link>
            <!--<a>Logout</a>-->
        </div>
  </nav>
</template>

<script>
  import {setAuthInHeader} from '../api'
  export default {
    computed:{
      isAuth(){
        return !!localStorage.getItem('token')
      }
    },
    methods:{
      logout(){
        delete localStorage.token
        setAuthInHeader(null)
        this.$router.push('/login')
      }
    }
  }
</script>

<style scoped>
  .header {
    flex: none;
    background-color: rgba(0,0,0,.15);
    height: 32px;
    padding: 4px;
  }
  .header a {
    display: block;
    height: 30px;
    line-height: 30px;
    text-decoration: none;
    color: rgba(255,255,255,.5);
  }
  .header-logo {
    position: absolute;
    left: 50%;
    top: 7px;
    margin-left: -30px;
    text-align: center;
    font-weight: bolder;
    font-size: 24px;
  }
  .header-logo a:hover,
  .header-logo a:focus {
    color: rgba(255,255,255,.9);
  }
  .header-auth {
    position: absolute;
    right: 15px;
    top: 5px;
  }
  .header-auth a {
    border-radius: 2px;
    padding: 0 10px;
    background-color: rgba(255,255,255, .5);
    color: white;
    transition: all .3s;
  }
  .header-auth a:hover,
  .header-auth a:focus {
    background-color: rgba(255,255,255, .3);
  }
</style>

~~~

### 23강 보드 목록 조회화면

Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="addBoard">
          Create new board...
        </a>
      </div>
    </div>
  </div>
</template>

<script>

  import {board} from '../api'

  export default {
    data() {
      return {
        loading: false,
        boards: [],
        error: ''
      }
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      addBoard() {
        console.log('addBoard()')
      }
    }
  }
</script>

<style scoped>
  .home-title {
    padding: 10px;
    font-size: 18px;
    font-weight: bold;
  }
  .board-list {
    padding: 10px;
    display: flex;
    flex-wrap: wrap;
  }
  .board-item {
    width: 23%;
    height: 100px;
    margin: 0 2% 20px 0;
    border-radius: 3px;
  }
  .board-item-new {
    background-color: #ddd;
  }
  .board-item a {
    text-decoration: none;
    display: block;
    width: 100%;
    height: 100%;
  }
  .board-item a:hover,
  .board-item a:focus {
    background-color: rgba(0,0,0, .1);
    color: #666;
  }
  .board-item-title {
    color: #fff;
    font-size: 18px;
    font-weight: 700;
    padding: 10px;
  }
  .board-item a.new-board-btn {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    height: 100px;
    width: inherit;
    color: #888;
    font-weight: 700;
  }
</style>

~~~

추가된건 updated다. Vuejs의 렌더링 사이클에 의해서 updated는 매번 호출이 된다. created 다음에 호출되고 데이터 객체에 변화가 감지되면 updated훅이 실행이 된다. 

### 24강 보드 추가하기

api/index.js

~~~javascript
import axios from 'axios'
import router from '../router'

const DOMAIN = 'http://localhost:3000'
const UNAUTHORIZED = 401
const onUnauthorized = () => {
  router.push(`/login?rPath=${encodeURIComponent(location.pathname)}`)
}

const request = (method, url, data) => {
  return axios({
    method,
    url: DOMAIN + url,
    data
  }).then(result => result.data)
    .catch(result => {
      const {status} = result.response
      if (status === UNAUTHORIZED) onUnauthorized()
      throw result.response
    })
}

//토큰 정보를 받아서 axios안에 기능을 추가한다.
//모든 리퀘스트를 날리기 전에 헤더 값을 토큰 정보로 설정하는 역할
export const setAuthInHeader = token => {
  axios.defaults.headers.common['Authorization'] = token ? `Bearer ${token}` : null;
}

const {token} = localStorage
if (token) setAuthInHeader(token)

export const board = {
  fetch() {
    return request('get', '/boards')
  },
  create(title){
    return request('post', '/boards', {title})
  }
}
export const auth = {
  login(email, password) {
    return request('post', '/login', {email, password})
  }
}

~~~

Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="addBoard">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" @close="isAddBoard=false" @submit="onAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        boards: [],
        error: '',
        isAddBoard: false
      }
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      addBoard() {
        this.isAddBoard = true;
      },
      onAddBoard(title){
        board.create(title).then(()=>{
          this.fetchData()
        })
      }
    }
  }
</script>

<style scoped>
  .home-title {
    padding: 10px;
    font-size: 18px;
    font-weight: bold;
  }
  .board-list {
    padding: 10px;
    display: flex;
    flex-wrap: wrap;
  }
  .board-item {
    width: 23%;
    height: 100px;
    margin: 0 2% 20px 0;
    border-radius: 3px;
  }
  .board-item-new {
    background-color: #ddd;
  }
  .board-item a {
    text-decoration: none;
    display: block;
    width: 100%;
    height: 100%;
  }
  .board-item a:hover,
  .board-item a:focus {
    background-color: rgba(0,0,0, .1);
    color: #666;
  }
  .board-item-title {
    color: #fff;
    font-size: 18px;
    font-weight: 700;
    padding: 10px;
  }
  .board-item a.new-board-btn {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    height: 100px;
    width: inherit;
    color: #888;
    font-weight: 700;
  }
</style>

~~~

AddBoard.vue

~~~javascript
<template>
  <Modal>
    <div slot="header">
      <h2>
        Create new board
        <a href="" class="modal-default-button"
           @click.prevent="close">&times;</a>
      </h2>
    </div>
    <div slot="body">
      <form id="add-board-form"
            @submit.prevent="addBoard">
        <input class="form-control" type="text" v-model="input" ref="input">
      </form>
    </div>
    <div slot="footer">
      <button class="btn" :class="{'btn-success': valid}" type="submit"
              form="add-board-form" :disabled="!valid">
        Create Board
      </button>
    </div>
  </Modal>
</template>

<script>
  import Modal from './Modal.vue'

  export default {
    components: {
      Modal
    },
    data() {
      return {
        input: '',
        valid: false
      }
    },
    watch: {
      input(v) {
        this.valid = v.trim().length > 0
      }
    },
    mounted() {
      this.$refs.input.focus()
    },
    methods: {
      close() {
        this.$emit('close')
      },
      addBoard() {
        this.$emit('close')
        this.$emit('submit', this.input)
      }
    }
  }
</script>

<style>
</style>

~~~

Modal.vue

~~~javascript
<template>
  <traansition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              default header
            </slot>
          </div>

          <div class="modal-body">
            <slot name="body">
              default body
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              default footer
              <button class="modal-default-button" @click="$emit('close')">
                OK
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </traansition>
</template>

<script>
    export default {
        name: "Modal"
    }
</script>

<style scoped>
  .modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    display: table;
    transition: opacity .3s ease;
  }

  .modal-wrapper {
    display: table-cell;
    vertical-align: middle;
  }

  .modal-container {
    width: 300px;
    margin: 0px auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
    transition: all .3s ease;
    font-family: Helvetica, Arial, sans-serif;
  }

  .modal-header h3 {
    margin-top: 0;
    color: #42b983;
  }

  .modal-body {
    margin: 20px 0;
  }

  .modal-default-button {
    float: right;
  }
</style>

~~~

Home 컴포넌트에서 버튼을 클릭하면 (모달을 이용한) AddBoard컴포넌트가 만들어진다. 그리고 AddBoard에서 발생한 이벤트를 받아주는 것은 또 Hom 컴포넌트 이다. 

즉, HomeComponent와 AddBoard컴포넌트 간에 중첩관계가 있다. 이렇게 중첩 관계가 하나 일때는 별 문제가 안되지만 UI가 복잡해질 수록 중첩관계는 더 깊어지게 된다. 그렇게 되면 데이터를 전달 해주는 단계가 많아지고 어플리케이션이 복잡해 질 수 있다.

한가지 더 AddBoard에서 입력한 것을 HomeComponent에서 받아서 Api를 호출하고 있는데 그렇게 하지 않고, 바로 AddBoard에서 호출할 수 도 있다. 그것이 덜 복잡하고 단순해보인다.

뿐만 아니라 서비스 전반에 사용될 수 있는 전역성격의 상태값이 있을 수 있다. 이러한 정보들을 좀 더 체계적으로 관리할 수 없을까?



### 25강 Vuex

전역 상태를 관리할 수 있는 Vuex라는 라이브러리를 살펴보자. 

Vuex 정의 = 상태 관리 패턴 + 라이브러리

어플리케이션의 모든 컴포넌트에 대한 종합 집중식 저장소 역할을 하며 예측 가능한 방식으로 상태를 변경할 수 있다. 그래서 위의 예제까지는 컴포넌트를 만들 때 컴포넌트 자체에 상태를 가지고 있었다. 하지만 Vuex는 모든 컴포넌트가 공유할 수 있는 종합 집중식 저장소를 만들 수 있다.



* 상태 (State) : 앱을 작동하는 원본 소스
* 뷰 : 상태의 선언적 맵핑
* 액션 : 뷰에서 사용자 입력에 대한 반응적으로 상태를 바꾸는 방법이다.



<단방향 데이터 흐름>

![](images/vuex-01.png)

그러나 공통의 상태를 공유하는 여러 컴포넌트가 있는 경우 단순함이 빠르게 저하된다.

1. 여러 뷰는 같은 상태에 의존한다. 
2. 서로 다른 뷰의 작업은 동일한 상태를 반영해야 할 수 있다.

첫번째 문제의 경우, 지나치게 중첩된 컴포넌트를 통과하는 prop는 장황할 수 있으며 형제 컴포넌트에서는 작동하지 않습니다. 두번째 문제의 경우 직접 부모/자식 인스턴스를 참조하거나 이벤트를 통해 상태의 여러 복사본을 변경 및 동기화 하려는 등의 해결 방법을 사용해야 합니다. 이러한 패턴은 모두 부서지기 쉽고 유지보수가 불가능한 코드로 빠르게 변경됩니다.

![](images/vuex-02.png)

### 26강 상태(State)

~~~javascript
npm i vuex
~~~

vuex는 단일 상태 트리를 사용한다. 즉, 하나의 상태를 가지고 있고 그상태는 트리형태로 되어있다. 이 단일 객체는 모든 애플리케이션 수준의 상태를 포함하며 "원본 소스" 역할을 합니다. 이는 각 애플리케이션마다 하나의 저장소만 갖게 된다는 것을 의미합니다. 단일 상태 트리를 사용하면 특정 상태를 쉽게 찾을 수 있으므로 디버깅을 위해 현재 앱 상태의 스냅 샷을 쉽게 가져올 수 있습니다.



src/store/index.js

~~~javascript
import Vue from 'vue';
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        isAddBoard: false
    }
})

export default store;
~~~



src/main.js

~~~javascript
import Vue from 'vue'
import router from './router'
import App from './App.vue'
import store from './store'

new Vue({
  el: '#app',
  router,
  store,
  ///entry point
  render: h => h(App)
})
~~~



home.vue

~~~javascript 
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="addBoard">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" @close="isAddBoard=false" @submit="onAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        boards: [],
        error: ''
      }
    },
    computed: {
      isAddboard(){
        return this.$store.state.isAddBoard;
      }
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      addBoard() {
        this.isAddBoard = true;
      },
      onAddBoard(title){
        board.create(title).then(()=>{
          this.fetchData()
        })
      }
    }
  }
</script>
~~~

mapState 라는 헬퍼 함수가 있다. 

컴포넌트가 여러 저장소 상태 속성이나 getter를 사용해야하는 경우 계산된 속성을 모두 선언하면 반복적이고 장황해집니다. 이를 처리하기 위해 우리는 계산된 getter 함수를 생성하는 `mapState` 헬퍼를 사용하여 키(코드) 입력을 줄일 수 있습니다.



Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="addBoard">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" @close="isAddBoard=false" @submit="onAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'
  import { mapState } from 'vuex'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        boards: [],
        error: ''
      }
    },
    computed: {
      ...mapState([
      'isAddBoard'
      ])
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      addBoard() {
        // this.isAddBoard = true;
      },
      onAddBoard(title){
        board.create(title).then(()=>{
          this.fetchData()
        })
      }
    }
  }
</script>
~~~

위 처럼 사용할 수 있다. 하지만 computed 속성에 mapState결과값을 설정해버리면 따로 computed 속성을 추가할 순 없다. 그래서 ES6의 해체문법(Sparead Operator)을 써줘서 다른 computed속성을 추가해서 사용 할 수 있다.



### 27강 변이(Mutation)

Vuex 저장소에서 실제로 상태를 변경하는 유일한 방법은 변이하는 것이다. Vuex의 변이는 이벤트와 매우 유사하다. 각 변이에는 타입 문자열 핸들러가 있다. 핸들러 함수는 실제 상태 수정을 하는 곳이며 첫번째 전달인자로 상태를 받는다.



src/store/index.js

~~~javascript
import Vue from 'vue';
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        isAddBoard: false
    },
    mutations: {
        SET_IS_ADD_BOARD(state, toggle){
            state.isAddBoard = toggle
        }
    }
})

export default store;
~~~



Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="addBoard">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" @close="isAddBoard=false" @submit="onAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'
  import { mapState } from 'vuex'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        boards: [],
        error: ''
      }
    },
    computed: {
      ...mapState([
      'isAddBoard'
      ])
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      addBoard() {
        //mutation을 실행시키려면 store의 commit 함수를 사용해야 한다.
        this.$store.commit('SET_IS_ADD_BOARD', true)
      },
      onAddBoard(title){
        board.create(title).then(()=>{
          this.fetchData()
        })
      }
    }
  }
</script>
~~~

mapState처럼 mutation도 헬퍼함수가 있다. computed 속성이 아닌 methods속성에 추가할 수 있다.



Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="SET_IS_ADD_BOARD(true)">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" @close="isAddBoard=false" @submit="onAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'
  import { mapState, mapMutations } from 'vuex'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        boards: [],
        error: ''
      }
    },
    computed: {
      ...mapState([
      'isAddBoard'
      ])
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      onAddBoard(title){
        board.create(title).then(()=>{
          this.fetchData()
        })
      }
    }
  }
</script>
~~~

 

AddBoard.vue

~~~javascript
<template>
  <Modal>
    <div slot="header">
      <h2>
        Create new board
        <a href="" class="modal-default-button"
           @click.prevent="SET_IS_ADD_BOARD(false)">&times;</a>
      </h2>
    </div>
    <div slot="body">
      <form id="add-board-form"
            @submit.prevent="addBoard">
        <input class="form-control" type="text" v-model="input" ref="input">
      </form>
    </div>
    <div slot="footer">
      <button class="btn" :class="{'btn-success': valid}" type="submit"
              form="add-board-form" :disabled="!valid">
        Create Board
      </button>
    </div>
  </Modal>
</template>

<script>
  import Modal from './Modal.vue'
  import { mapMutations } from 'vuex'

  export default {
    components: {
      Modal
    },
    data() {
      return {
        input: '',
        valid: false
      }
    },
    watch: {
      input(v) {
        this.valid = v.trim().length > 0
      }
    },
    mounted() {
      this.$refs.input.focus()
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      addBoard() {
        this.SET_IS_ADD_BOARD(false)
        this.$emit('submit', this.input)
      }
    }
  }
</script>
~~~



### 28강 액션(Action)

액션은 변이와 유사합니다. 몇가지 다른 점은,

- 상태를 변이시키는 대신 액션으로 변이에 대한 커밋을 합니다. **액션에서 직접 상태를 변경시키는 것이 아니라 액션에서 mutation을 호출하고 변이가 상태를 변경하도록 한다.**
- 작업에는 임의의 비동기 작업이 포함될 수 있습니다. **mutation는 동기만 가능하다.**



액션함수는 context라는 종합 객체를 받는다. 종합 객체라는 것은 이 안에 state와 getter같은 것들이 통합되어서 들어오기 때문이다.

이렇게 등록한 액션은 스토어의 dispatch함수를 통해서 실행한다. mutation은 commit을 사용, 액션은 dispatch함수를 사용한다.



action은 내부적으로는 변이함수를 실행하거나 api콜(비동기 작업)을 실행한다.



src/store/index.js

~~~javascript
import Vue from 'vue';
import Vuex from 'vuex'
import api from '../api'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        isAddBoard: false
    },
    mutations: {
        SET_IS_ADD_BOARD(state, toggle){
            state.isAddBoard = toggle
        }
    },
    actions: {
        ADD_BOARD(_, {title}){
            return api.board.create(title)
        }
    }
})

export default store;
~~~



AddBoard.vue

~~~javascript
<template>
  <Modal>
    <div slot="header">
      <h2>
        Create new board
        <a href="" class="modal-default-button"
           @click.prevent="SET_IS_ADD_BOARD(false)">&times;</a>
      </h2>
    </div>
    <div slot="body">
      <form id="add-board-form"
            @submit.prevent="addBoard">
        <input class="form-control" type="text" v-model="input" ref="input">
      </form>
    </div>
    <div slot="footer">
      <button class="btn" :class="{'btn-success': valid}" type="submit"
              form="add-board-form" :disabled="!valid">
        Create Board
      </button>
    </div>
  </Modal>
</template>

<script>
  import Modal from './Modal.vue'
  import { mapMutations } from 'vuex'

  export default {
    components: {
      Modal
    },
    data() {
      return {
        input: '',
        valid: false
      }
    },
    watch: {
      input(v) {
        this.valid = v.trim().length > 0
      }
    },
    mounted() {
      this.$refs.input.focus()
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      addBoard() {
        this.SET_IS_ADD_BOARD(false)
        this.$emit('submit')
        this.$store.dispatch('ADD_BOARD', this.input)
      }
    }
  }
</script>
~~~



Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="SET_IS_ADD_BOARD(true)">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" @close="isAddBoard=false" @submit="onAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'
  import { mapState, mapMutations } from 'vuex'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        boards: [],
        error: ''
      }
    },
    computed: {
      ...mapState([
      'isAddBoard'
      ])
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      fetchData() {
        this.loading = true
        board.fetch()
          .then(data => {
            this.boards = data.list
          })
          .finally(_=> {
            this.loading = false
          })
      },
      onAddBoard(title){
          this.fetchData()
      }
    }
  }
</script>
~~~



actions도 역시 헬퍼함수를 제공한다.   methods에서 사용할 수 있다.



AddBoard.vue

~~~javascript
<template>
  <Modal>
    <div slot="header">
      <h2>
        Create new board
        <a href="" class="modal-default-button"
           @click.prevent="SET_IS_ADD_BOARD(false)">&times;</a>
      </h2>
    </div>
    <div slot="body">
      <form id="add-board-form"
            @submit.prevent="addBoard">
        <input class="form-control" type="text" v-model="input" ref="input">
      </form>
    </div>
    <div slot="footer">
      <button class="btn" :class="{'btn-success': valid}" type="submit"
              form="add-board-form" :disabled="!valid">
        Create Board
      </button>
    </div>
  </Modal>
</template>

<script>
  import Modal from './Modal.vue'
  import { mapMutations, mapActions } from 'vuex'

  export default {
    components: {
      Modal
    },
    data() {
      return {
        input: '',
        valid: false
      }
    },
    watch: {
      input(v) {
        this.valid = v.trim().length > 0
      }
    },
    mounted() {
      this.$refs.input.focus()
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      ...mapActions([
        'ADD_BOARD'
      ]),
      addBoard() {
        this.SET_IS_ADD_BOARD(false)
        this.ADD_BOARD({title: this.input})
        this.$emit('submit')
      }
    }
  }
</script>

<style>
</style>

~~~



살펴본것 처럼 vuex에는 state, mutation, action이 있다. 이 외에도 Getters가 있다. Getter는 상태값을 받아오는 함수이다. 필요에 따라  사용하면 된다.

### 29강 Vuex적용 - 보드목록조회

src/store/index.js

~~~javascript
import Vue from 'vue';
import Vuex from 'vuex'
import * as api from '../api'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        isAddBoard: false,
        boards: []
    },
    mutations: {
        SET_IS_ADD_BOARD(state, toggle){ 
            state.isAddBoard = toggle
        },
        SET_BOARDS(state, boards){
            state.boards = boards
        }
    },
    actions: {
        ADD_BOARD(_, {title}){
            return api.board.create(title)
        },
        /* actions의 첫번째 인자로는 컨텍스트 객체가 오는데 컨텍스트 객체 중에 commit 이라는 함수를 쓴다.
        내부적으로 SET_BOARDS를 이용해 boards 상태를 갱신하도록 한다. */
        FETCH_BOARDS({ commit }){
            return api.board.fetch().then(data => {
                commit('SET_BOARDS', data.list)
            })
        }
    }
})

export default store;
~~~



Home.vue

~~~javascript
<template>
  <div>
    <div class="home-title">Personal Boards</div>
    <div class="board-list" ref="boardList">
      <div class="board-item" v-for="b in boards" :key="b.id"
           :data-bgcolor="b.bgColor" ref="boardItem">
        <router-link :to="`/b/${b.id}`">
          <div class="board-item-title">{{b.title}}</div>
        </router-link>
      </div>
      <div class="board-item board-item-new">
        <a class="new-board-btn" href="" @click.prevent="SET_IS_ADD_BOARD(true)">
          Create new board...
        </a>
      </div>
    </div>
    <AddBoard v-if="isAddBoard"/>
  </div>
</template>

<script>
  import {board} from '../api'
  import AddBoard from './AddBoard'
  import { mapState, mapMutations, mapActions} from 'vuex'

  export default {
    components:{
      AddBoard
    },
    data() {
      return {
        loading: false,
        error: ''
      }
    },
    computed: {
      ...mapState([
      'isAddBoard',
      'boards'
      ])
    },
    created() {
      this.fetchData()
    },
    updated() {
      this.$refs.boardItem.forEach(el => {
        el.style.backgroundColor = el.dataset.bgcolor
      })
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      ...mapActions([
        'FETCH_BOARDS'
      ]),
      fetchData() {
        this.loading = true
        this.FETCH_BOARDS().finally(_=> {
          this.loading = false
        })
      }
    }
  }
</script>
~~~



AddBoard.vue

~~~Javascript
<template>
  <Modal>
    <div slot="header">
      <h2>
        Create new board
        <a href="" class="modal-default-button"
           @click.prevent="SET_IS_ADD_BOARD(false)">&times;</a>
      </h2>
    </div>
    <div slot="body">
      <form id="add-board-form"
            @submit.prevent="addBoard">
        <input class="form-control" type="text" v-model="input" ref="input">
      </form>
    </div>
    <div slot="footer">
      <button class="btn" :class="{'btn-success': valid}" type="submit"
              form="add-board-form" :disabled="!valid">
        Create Board
      </button>
    </div>
  </Modal>
</template>

<script>
  import Modal from './Modal.vue'
  import { mapMutations, mapActions } from 'vuex'

  export default {
    components: {
      Modal
    },
    data() {
      return {
        input: '',
        valid: false
      }
    },
    watch: {
      input(v) {
        this.valid = v.trim().length > 0
      }
    },
    mounted() {
      this.$refs.input.focus()
    },
    methods: {
      ...mapMutations([
        'SET_IS_ADD_BOARD'
      ]),
      ...mapActions([
        'ADD_BOARD',
        'FETCH_BOARDS'
      ]),
      addBoard() {
        this.SET_IS_ADD_BOARD(false)
        this.ADD_BOARD({title: this.input}).then(()=>{
          this.FETCH_BOARDS()
        })
      }
    }
  }
</script>

<style>
</style>

~~~



### 이 후 내용은 코드로...

















