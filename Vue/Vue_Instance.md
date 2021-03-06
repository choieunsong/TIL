# Vue Instance 생성
```html
<script>
    new Vue({
        el: '#app',
        data: {
            message: '첫번째 vue.js 앱입니다'
        }
    })
</script>
```
+ el: Vue가 적용될 요소 지정. css selector or html element
+ data: vue에서 사용되는 정보 저장, 객체 또는 함수의 형태
+ template: 화면에 표시할 html, css 등의 마크업 요소 정의
+ methods: 화면 로직 제어와 관계된 method를 정의하는 속성. 마우스 클릭 이벤트 처리와 같이 화면의 전반적인 이벤트와 화면 동작과 관련된 로직을 추가
+ created: 뷰 인스턴스가 생성되자 마자 실행할 로직을 정의

# Vue Instance의 유효 범위
Vue Instance를 생성하면 html의 특정 범위 안에서만 옵션 속성들이 적용된다.
+ 인스턴스가 화면에 적용되는 과정
1. 뷰 라이브러리 파일 로딩
2. 인스턴스 객체 생성(옵션 속성 포함)
3. 특정 화면 요소에 인스턴스를 붙임
4. 인스턴스 내용이 화면 요소로 전환
5. 변환된 화면 요소를 사용자가 최종 확인


## MVVM 패턴
model <--> view model <--> view
+ model: 순수 자바스크립트 객체
+ view: 웹페이지의 DOM
+ viewModel: vue의 역할
기존에는 자바스크립트로 view에 해당하는 DOM에 접근하거나 수정하기 위해 JQuery같은 라이브러리 이용. Vue는 view와 Model을 연결하고 자동으로 바인딩하므로 양방향 통신을 가능하게 함


# Vue 인스턴스 라이프 사이클
크게 생성되고(create), DOM에 부착(mount)되고, 업데이트(update)되고, 없어진다(destroy). 시간순대로 많이 사용하는 훅(hook)
+ beforeCraete: 뷰 인스턴스가 생성되고 호출. 컴포넌트가 DOM에 추가되기도 전이라 접근 불가. `data`, `methods` 접근 불가
+ created: 뷰 인스턴스가 생성된 후 데이터 설정까지 끝나면 호출. 인스턴스가 화면에 부착되기 전이므로 DOM 요소 접근 불가. `data`, `computed`,`methods`,`watch` 접근 가능. 서버와 통신 로직이나 이벤트 리스너 선언을 여기서 하는게 좋다.
+ beforeMount: DOM에 부착하기 직전에 호출됨. 이 단계에서 템플릿이 있는지 없는 지 확인한 후 템플릿을 렌더링한 상태이므로 가상 DOM이 생성되어 있으니 실제 DOM에 부착되진 않음
+ mounted: 지정된 element에 뷰인스턴스 데이터가 마운트 된 후 호출. 템플릿 속성에 정의한 화면 요소에 접근할 수 있어 화면 요소를 제어하는 로직 수행
+ beforeUpdate: 데이터가 변경될 때 가상 DOM이 랜더링, 패치 되기 전에 호출
+ updated: 가상 DOM을 렌더링 하고 실제 DOM이 변경된 이후에 호출됨
+ beforeDestroy: 뷰인스턴스가 제거되기 전에 호출. 이벤트 리스너 해제 등의 작업을 하기에 적합함
+ destroyed: 뷰인스턴스가 제거된 후에 호출

# Template
뷰의 템플릿 문법이랑 뷰로 화면을 조작하는 방법을 의미한다. 템플릿 문법은 크게 `데이터 바인딩`과 `디렉티브`로 나뉜다.

## 데이터 바인딩
데이터 바인딩은 뷰인스턴스에서 정의한 속성을 화면에 표시하는 방법. 보편적인 방법으로 `mustache tag`가 있다.
```html
<div>{{message}}</div>

new Vue({
    data:
        message: 'Hello Vue.js'
})
```
+ mustache tag는 html이 아닌 일반 텍스트로 해석된다.
  + `<v-text>`도 일반 텍스트
+ html로 인식하게 하려면 `<span v-html="message"></span>`
+ vue.js는 모든 데이터 바인딩 내에서 javascript 표현식의 모든 기능을 지원함(단 단일식만) `{{message.split('').reverse().join('')}}`

## 디렉티브
디렉티브는 뷰로 화면의 효소를 더 쉽게 모아 조작하기 위한 문법. 화면 조작에 자주 사용되는 디렉티브를 따로 모아두었다.
+ v- 접두사가 있는 특수 속성
+ 디렉티브 속성 값은 단일 javascript 표현식이 된다.(v-for 제외)
+ 디렉티브의 역할은 표현식의 값이 변경될 때 사이드 이펙트를 반응적으로 DOM에 적용

## v-model
+ 양방향 바인딩 처리를 위해서 사용(form의 input, textarea)
```html
<input type="text" v-model="msg">
...
<script>
    new Vue({
        el: '#app',
        data:{
            msg: 'hello~'
        }
    })
</script>
```

## v-bind
+ 엘리먼트의 속성과 바인딩 처리를 위해서 사용
+ v-bind는 약어로 ':'로 사용 가능
+ `class`와 `style`을 제어할 때 보통 사용
+ 2.6.0버전부터 Javascript 표현식을 대괄호로 묶어 디렉티브의 아규먼트로 활용할 수 있음 => 동적 전달인자
    ```html
    <a v-bind:[attributeName]="url">...</a>
    ```
    여기서 `attributeName`가 `'href'`라는 값을 가진다면 `v-bind:href`와 동일함
```html
<div v-bind:id="idValue">v-bind 지정 연습</div>
<button v-bind:[key]="btnId">버튼</button>

data:{
    idValue: 'test-id',
    key: 'id'
}
```

## v-show, v-if
+ 조건에 따라 엘리먼트를 화면에 렌더링
+ 차이점
  +  렌더링
     +   v-if: false일 경우 렌더링 안함
     +   v-show: 항상 렌더링
  + false일 경우
      + v-if: 엘리먼트 삭제
      + v-show: display:none 적용
  + template 지원
      + v-if: 지원함
      + v-show: 지원안함

## v-for
+ 배열이나 객체의 반복에 사용
```html
<li v-for="(item, index) in items"> 
    {{parentMessage}} - {{index}} - {{item.message}}
</li>

data:{
    items: [
        {message: 'Foo'},
        {message: 'Foo'}
    ]
}
```

## v-cloak
+ 뷰 인스턴스가 준비될 때까지 mustache 바인딩을 숨기는데 사용
+ [v-cloak] {display: none}과 같은 css 규칙과 함께 사용
+ 뷰 인스턴스가 준비되면 v-cloak는 제거됨
```html
<style>
        [v-cloak]::before {
            content: '로딩중...'
        }
        [v-cloak] > * {
            display: none;
        }
</style>
<div id="app">
        <h1>1 일반. - {{msg}}</h1>
        <div v-cloak>
            <h1>2 v-cloak. - {{msg}}</h1>
        </div>
    </div>
    <script>
        setTimeout(function () {
            new Vue({
                el: "#app",
                data: {
                    msg: "hello"
                }
            })
        }, 3000);
    </script>
```
+ 결과
  1. 1.일반 - {{msg}} 2.로딩중
  2. 1.일반 - hello 2.v-cloak. -hello

## Vue method
+ method 안에서 data를 "this.데이터이름"으로 접근 가능
  ```html
  methods:{
      helloEng(){
          return "hello " + this.name
    }
  }
  ```

## Vue filter
+ 중괄호 보간법: `{{ message | capitalize}}`
+ v-bind 표현: `<div v-bind: id="rawId | formatId"></div>`
```html
<input type="text" v-model="msg1"> 
...
<h3>{{msg1 | price}}</h3>
...
//전역필터
Vue.filter('price', (value) => {
    if(!value) return value;
    return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
})
```

## Vue Computed
+ 특정 데이터의 변경사항을 실시간으로 처리
+ 캐싱을 이용하여 데이터 변경이 없을 경우 캐싱된 데이터 반환
+ setter와 getter를 직접 지정할 수 있음
+ computed 속성은 한번 실행(캐싱), method로 하면 호출될때마다 실행
+ 
```html
var vm = new Vue({
    el: '#example',
    data: {
        message: '안녕하세요'
    },
    computed: {
        reversedMessage: function(){
            return this.message.split(').reverse().join('');
        }
    }
})
```

## Vue watch
+ 뷰 인스턴스의 특정 property가 변경될 때 실행할 콜백 함수 설정
```html
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

### Computed vs Watch
+ `watch`는 감시할 데이터를 지정하고 그 데이터가 바뀌면 이런 함수를 실행하라는 방식의 **명령형 프로그래밍**
+ `computed`는 계산해야 하는 목표 데이터를 정의하는 방식으로 **선언형 프로그래밍**
+ `watch`보다 `computed`를 자주 써라


## v-on
+ DOM 이벤트를 듣고 트리거될 때 javascript 실행, 혹은 메서드 핸들링
```html
<div id="example-2">
  <!-- `greet`는 메소드 이름으로 아래에 정의되어 있습니다 -->
  <button v-on:click="greet">Greet</button>
</div>

var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 메소드는 `methods` 객체 안에 정의합니다
  methods: {
    greet: function (event) {
      // 메소드 안에서 사용하는 `this` 는 Vue 인스턴스를 가리킵니다
      alert('Hello ' + this.name + '!')
      // `event` 는 네이티브 DOM 이벤트입니다
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
```
### v-on 이벤트 수식어
+ 클릭 이벤트 전파 중단
  `<a v-on:click.stop = "doThis"></a>`
+ 제출 이벤트가 페이지를 다시 로드 하지 않음
  `<a v-on:submit.prevent = "onSubmit"></a>`
+ 수식어는 체이닝 가능
  `<a v-on:click.stop.prevent = "doThat"></a>`
+ 단순히 수식어만 사용할 수 있다
  `<a v-on:submit.prevent = "doThat"></a>`
+ `v-on`은 `@`로 축약 가능

## 키 수식어
뷰는 키 이벤트를 수신할 때 `v-on:keyup.enter="submit"`이렇게 키 수식어 추가 가능
+ enter, tab, delete, esc, space, up, down, left, right

## ref, $refs
+ 뷰에서 $refs 속성을 이용해 DOM에 접근할 수 있다.
+ 다만 뷰의 가장 중요한 목적 중 하나는 개발자가 DOM을 다루지 않게 하는 것이므로 됟로고 ref를 사용하는 것을 피하는 게 좋다
```html
<input type="text> v-model="id" ref="id">

methods:{
    search(){
        if(this.id.length == 0){
            this.$refs.id.focus();
            return;
        }
        console.log(this.$rets.id.value);
    }
}
 ```

## form - checkbox
+ 하나의 체크박스는 단일 boolean 값을 갖는다.
```html
<input type="checkbox" id="smsYN" v-model"sns" true-value="Y" false-value="N">  
```
+ 배열일 경우 checkbox는 value 속성을 가져온단
  
## form - radio
+ value의 속성 값 관리
```html
<input ype="radio" id="gwangju" value="광주" v-model="ckArea">
<input ype="radio" id="seoul" value="서울" v-model="ckArea">
<input ype="radio" id="daejeon" value="대전" v-model="ckArea">
<input ype="radio" id="busan" value="부산" v-model="ckArea">
data:{
    ckArea: '광주'  //광주가 체크됨
}
```
+ select box도 마찬가지

## form - 수식어
+ lazy: change 이벤트 이후에 동기화
+ number: 사용자의 입력이 자동으로 숫자로 형 변화 되기를 원하면 v-model의 input에 number 수식어를 추가하면 된다.
+ trim: v-model input이 자동으로 trim 가능
  `<input v-model.trim="msg">`