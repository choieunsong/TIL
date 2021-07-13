# Component
+ Vue의 가장 강력한 기능 중 하나
+ HTML Element를 확장하여 재사용 가능한 코드를 캡슐화
+ Vue Component는 Vue Instance이기도 하기 때문에 모든 옵션 객체를 사용
+ Life Cycle Hook 사용 가능
+ 전역 컴포넌트와 지역 컴포넌트로 나뉨


+ 전역 컴포넌트
```html
<div id="app">
    <app-header></app-header>
</div>

Vue.component('app-header', {
    template: '<h1>Header Component</h1>'
})
```

+ 지역 컴포넌트
```html
var appHeader = {
    template = '<h1>Header Component</h1>'
}
new Vue({
    components: {
        'app-header': appHeader
    }
})
```

# Component간 통신 방법
뷰 컴포넌트는 각각 고유한 데이터 유효 범위를 갖는다. 따라서, 컴포넌트 간에 데이터를 주고 받기 위해선 `이벤트`, `props`를 사용한다.
+ 부모에서 자식: props라는 특별한 속성을 전달
+ 자식에서 부모: event로만 전달 가능

# props
```html
<!-- 하위 컴포넌트: childComponent -->
var childComponent = {
    props: ['propsdata'],
    template: '<p>{{propsdata}}</p>'
}

<!-- 상위컴포넌트: root 컴포넌트 -->
new Vue({
    el: '#app',
    components: {
        'child-component': childComponent
    },
    data:{
        message: 'hello vue.js'
    }
})
<div id="app">
    <child-component v-bind:propsdata="mesage"></child-component>
</div>
```

## 동적 props
+ v-bind를 사용하여 부모의 데이터에 props를 동적으로 바인딩 할 수 있다.
+ 데이터가 상위에서 업데이트 될 때 마다 하위 데이터로도 전달된다.
```html
<div>
    <input v-model="parentMsg">
    <br>
    <child :my-message="parentMsg"></child>
</div>
```

+ 오브젝트의 모든 속성을 전달할 경우 `v-bind:prop-name` 대신 v-bind만 작성함으로써 모든 속성을 prop으로 전달할 수 있다.
```html
post:{
    id: 1,
    title: 'My Journey with Vue'
}
<blog-post v-bind="post"></blog-post>
```

# 이벤트
+ props와 달리 이벤트는 자동 대소문자 변환을 제공하지 않음
+ v-on 이벤트 리스너는 항상 자동으로 소문자 변환되기 때문에 `myEvent`는 `myevent`로 변환된다. 그러므로 이벤트 이름은 kebab-case 사용이 권장됨
```html
<!-- 이벤트 발생 -->
vm.$emit('이벤트명', [...파라미터]);
ex) vm.$emit('spped', 100);

<!-- 이벤트 수신 -->
vm.$on('이벤트명', 콜백함수(){});
ex) vm.$on('speed', function(speed)());
```
```html
<script>
    Vue.component('Subject', {
    template: '<button v-on:click="addCount">{{title}} - {{ count }}</button>',
    props: ['title'],
    data: function () {
        return {
        count: 0,
        };
    },
    methods: {
        addCount: function () {
        this.count += 1;
        // 부모 v-on:이름 에 해당하는 이름의 이벤트를 호출
        this.$emit('addtotcount');
        },
    },
    });

    new Vue({
    el: '#app',
    data: {
        total: 0,
    },
    methods: {
        addTotalCount: function () {
        this.total += 1;
        },
    },
    });
</script>
```

## 비 상하위간 통신
+ 비어 있는 vue instance 객체를 event bus로 사용
+ 복잡해질 경우 `vuex`사용 권장
```html
var bus = new Vue();
vus.$emit('id-selected', 1);
bud.$on('id-selected', function(id){});
```
```html
<body>
    <div id="app">
      <my-count></my-count>
      <log></log>
    </div>
    <template id="myCount">
      <div>
        <input type="text" v-model.number="count" @keyup.enter="send" />
        <button @click="send">보내기</button>
      </div>
    </template>
    <template id="log">
      <div>
        <h2>{{count}}</h2>
        <ul>
          <li v-for="msg in list">{{msg}}</li>
        </ul>
      </div>
    </template>
    <script>
      const bus = new Vue();
      Vue.component('my-count', {
        template: '#myCount',
        data() {
          return {
            count: 0,
          };
        },
        methods: {
          send() {
            bus.$emit('updateLog', this.count);
            this.count = '';
          },
        },
      });

      Vue.component('Log', {
        template: '#log',
        data() {
          return {
            count: 0,
            list: [],
          };
        },
        methods: {
          updateLog(data) {
            this.count += data;
            this.list.push(`${data}을 받았습니다.`);
          },
        },
        created: function () {
          bus.$on('updateLog', this.updateLog);
        },
      });

      new Vue({
        el: '#app',
      });
    </script>
  </body>
```