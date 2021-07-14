# Vuex
+ Vue.js application에 대한 상태관리패턴 + 라이브러리이다.
+ 어플리케이션의 모든 component들의 중앙 저장소 역할을 한다(데이터 관리)
+ 상위(부모), 하위(자식)의 단계가 많이 복잡해 진다면 데이터 전달이 복잡해짐 => store에서 관리하자!
  
## Vuex 저장소 개념
+ State: 단일 상태 트리를 사용. application마다 하나의 저장소를 관리(data)
+ Getters: Vue Instance의 Computed와 같은 역할. State를 기반으로 계산(computed)
+ Mutations: State의 상태를 변경하는 유일한 방법(methods)
+ Actions: 상태를 변이 시키는 대신 액션으로 변이에 대한 커밋 처리(비동기 methods)

```
Vue.use(Vuex);  //Vue Instance에 Vuex를 설정
```
`main.js`에서 App 실행
```
import Vue from "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";

Vue.config.productionTip = false;

new Vue({
  router,
  store,
  render: (h) => h(App),
}).$mount("#app");

```
보통 `store` > `index.js`에 있음

### State에 접근하는 방식
```
this.$store.state.data_name
```

### 저장소 - Getters
+ component가 vuex의 state를 직접 접근하는 코드가 중복되는 경우 
```
// getter 사용 부분
this.$store.getters.countMsg;

// index.js
getters:{
    countMsg(state){
        state.count += 1;
    }
}
```

### Store - mapGetters
+ getters를 조금 더 간단하게 호출할 수 있음
+ `mapGetters` 헬퍼는 저장소 getter를 로컬 계산된 속성에 매핑한다.
+ `this.$state.getters.countMsg`로 호출할 것을 `{{countMsg}}`로 호출 가능
```
getters: {
    // 복잡한 계산식을 처리 : computed
    countMsg(state) {
      // return state.count + '번 호출됨';
      let msg = '10번보다 ';
      if (state.count > 10) {
        msg += '많이 ';
      } else {
        msg += '적게 ';
      }
      return msg + ' 호출됨(' + state.count + ')';
    },
    msg1(state) {
      return 'msg1 : ' + state.count;
    },
    msg2(state) {
      return 'msg2 : ' + state.count;
    },
    msg3(state) {
      return 'msg3 : ' + state.count;
    },
  },

import { mapGetters } from 'vuex';
export default{
    computed: {
        ...mapGetters(["countMsg", "msg1", "msg2", "msg3"]),
    }
}
```

### Store - Mutations
+ State 값을 변경하기 위해서 사용함
+ 각 컴포넌트에서 State의 값을 직접 변경하는 것은 권장하지 않는 방식
+ State의 값의 추적을 위해 동기적 기능에 사용
+ Mutations는 직접 호출이 불가능하고 store.commit('정의된 이름')으로 호출
+ 첫번째 인자로 들어오는 `state`는 상태
```
//index.js에 저장된 것
export default new Vuew.Store({
    mutations:{
        ADD_ONE(state){
            state.count += 1;
        }
    }
})

//호출 방법
this.$store.commit('ADD_ONE');
```
+ `payload`를 가진 커밋
+ 대부분의 페이로드는 여러 필드를 포함할 수 있는 객체
```
//index.js에 저장된 것
export default new Vuew.Store({
    mutations:{
        increament(state, payload){
            state.count += payload.amount;
        }
    }
})

//호출 방법
$state.commit('increment', {
    amount: 10
})
```

### mapMutations
+ `mapGetters`처럼 `mutations`를 더 간단하게 호출
```
import {mapMutations} from 'vuex';

methods: {
    ...mapMutations({
        addOne: 'addOne',
        addMTenCount: 'addTenCount',
        addMObjCount: 'addObjCount',
    }),
    addCount: function() {
      this.count += 1;
      // this.$store.commit('addOne');
      this.addMOne();
    },
}
```

### Store - Actions
+ 비동기 작업의 결과를 적용하려고 할 때 사용
+ Mutations는  상태 관리를 위해 동기적으로 처리하고 비동기적인 처리는 Actions가 담당
+ Actions는 비동기 로직의 처리가 종료되면 Mutations를 호출
![image](https://user-images.githubusercontent.com/24693833/125636549-443faf6d-5c4d-4fa2-8543-270730c770ee.png)
+ Actions는 인자로 컨텍스트 객체를 받는다. 그래서 `context.commit`을 호출하여 mutations를 커밋하거나, `context.state`와 `context.getters`를 통해 상태와 getters에 접근할 수 있음
```
// 액션은 store.dispatch 메소드로 시작됨 
asyncCount() {
    this.$store.dispatch("asyncAddOne");
},

//비동기 commit. 2초 뒤에 mutations ADD_ONE 실행
actions: {
    asyncAddOne(context) {
        console.log("Actions asycnAddOne call");
        setTimeout(() => {
            context.commit('ADD_ONE');
        }, 2000);
    },
},
mutations: {
    ADD_ONE(state) {
      console.log("Mutations ADD_ONE call");
      state.count += 1;
    },
    ADD_TEN_COUNT(state, payload) {
      state.count += payload;
    },
    ADD_OBJ_COUNT(state, payload) {
      state.count += payload.num;
    },
  },
```

### mapActions
```
import {mapMutations, mapActions} from 'vuex';

methods:{
    ...mapActions(['asyncAddOne']);
}
```