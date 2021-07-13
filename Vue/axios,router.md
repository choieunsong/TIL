# axios
+ Vue에서 권고하는 http 라이브러리
+ Promise 기반의 http 통신 라이브러리
+ cdn 방식
```
<script scr="https://unpkg.com/axios/dist/axios.min.js></script>
```
+ npm 방식
```
npm install axios
```

## Promise
+ 비동기 로직 처리를 위한 자바스크립트 라이브러리
+ 성공하면 resolve 호출, 실패하면 reject 호출
```javascript
 // 객체 생성하기
const promise = new Promise((resolve, reject) => {
    console.log('Promise');
    // resolve('resolve'); // >> then 부분 실행
    reject('reject'); // >> catch 부분 실행
});

promise
.then((data) => { 
    console.log("then: " + data);
})
.catch((data) => {
    console.log("catch: " + data);
});
```

## axios API
+ 유형
  + axios.get('URL 주소').then().catch()
  + axios.post('URL 주소).then().catch()
  + axios({옵션속성})

```javascript
//POST request
axios({
    methods: 'post',
    url: '/user',
    data: {
        name: '홍길동',
        userid: 'ssafy'
    }
})

//GET requset
axios({
    method: 'get',
    url: '/user/ssafy',
    responseType: 'json'
})
.then(function(response){
    //logic
})

//PUT request
axios({
    method: 'put',
    url: '/board/10',
    data: {subject: '안녕하세요', content: '오늘도 즐거운 하루 되세요'}
})

//DELETE request
axios({
    method: 'delete',
    url: '/board/10'
});
```

# vue-router
+ 라우팅: 웹 페이지 간의 이동 방법
+ vue.js의 공식 라우터
+ 라우터는 컴포넌트와 매핑
+ vue를 이용한 SPA를 제작할 때 유용하다
+ url에 따라 컴포넌트를 연결하고 설정된 컴포넌트를 보여준다.
+ cdn방식
```html
<script src="https://cdn.jsdeliver.net/npm/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```
* npm 방식
```
npm install vue-router
```

# vue-router 연결
+ `routes`옵션과 함께 router instance 생성
```html
const Main = {
    template: '<div>메인페이지</div>',
};
const Board = {
    template: '<div>자유게시판</div>',
};
const router = new VueRouter({
    routes: [
        {
            path: '/',
            component: Main,
        },
        {
            path: '/board',
            component: Board,
        }
    ]
})
```
+ 네비게이션을 위해 router-link 컴포넌트를 사용
+ 속성은 'to' prop을 사용
+ 기본적으로 `<router-link>`는 `<a>`태그로 렌더링
```html
<!-- 네비게이션 -->
<router-link to="/">Home</router-link>
<router-link to="/board">게시판</router-link>

<!-- 컴포넌트 렌더링 -->
<router-view></router-view>
```

## 라우터 정보
```html
<!-- 전체 라우터 정보 -->
<router-link to="/board/30">30번 게시글</router-link>

<!-- 현재 호출된 해당 라우터 정보 -->
this.$route
this.$route.params.no
this.$route.path
```

# 동적 라우트 매칭
+ 패턴이 있는 라우트, parameter가 있는 라우트의 경우 `:to`(동적 세그먼트 콜론)로 param 넘긴다
+ `:to` 값은 `this.$route.params`로 표시된다
```html
<!-- Board 부분에서 BoardView 호출 -->
<li v-for="i in 5">
    <router-link :to="'/board/' + i">{{i}}번 게시글</router-link>
</li>

<!-- BoardView 호출 -->
export default {
    template: `<div>{{no}}번 게시물 정보</div>`
}
```

# 프로그래밍 방식 라우터
+ Veu 인스턴스 내부에서 라우터 인스턴스에 $router로 액세스 할 수 있음. `this.$router.push`
```
// 리터럴 string
router.push('home')

// object
router.push({ path: 'home' })

// 이름을 가지는 라우트
router.push({ name: 'user', params: { userId: 123 }})

// 쿼리와 함께 사용, 결과는 /register?plan=private 입니다.
router.push({ path: 'register', query: { plan: 'private' }})
```
+ 이것 외에도 `router.replace(location)`도 있음.
+ `router.go(n)`로 히스토리 스택 앞뒤로 이동 가능. 
+ 1이면 한 단계 앞으로 감. `window.history.forward()`와 동일
+ -1이면 한 단계 뒤로 감. `window.history.back()`과 동일
+ 자세한 사항은 Vue Router 공식 문서를 참고하자. 이런게 있다

# 중척된 라우트
+ 라우터 링크를 타고 들어갔는데 거기서 또 중첩된 컴포넌트가 있을 때 사용
+ 그럴 땐 url 세그먼트와 중첩 된 컴포넌트의 구조를 동일하게 만든다.
+ app 밑에 게시판(board)가 있고 게시판 밑에 글들(boardView)가 있을 때 게시판이 `<router-view>`가 되야 함
```
// 라우터 객체 생성
const router = new VueRouter({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'main',
      component: Main,
    },
    {
      path: '/board',
      name: 'board',
      component: Board,
      redirect: '/board/list',
      children: [
        {
          path: 'list',
          name: 'boardlist',
          component: BoardList,
        },
        {
          path: 'write',
          name: 'boardwrite',
          component: BoardWrite,
        },
        {
          path: 'detail/:no',
          name: 'boardview',
          component: BoardView,
        },
        {
          path: 'update/:no',
          name: 'boardupdate',
          component: BoardUpdate,
        },
      ],
    },
    {
      path: '/qna',
      name: 'qna',
      component: QnA,
    },
    {
      path: '/gallery',
      name: 'gallery',
      component: Gallery,
    },
  ],
});
```
+ history란 vue-router를 이용해 페이지를 다시 로드할 수 있게 하는 것.
+ 기본은 hash mode로 앞뒤 이동했을 때 페이지가 다시 로드되지 않음