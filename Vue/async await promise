### Async, Await에서 Promise로 결과값 받기
```javascript
 // 관심 클래스 등록여부 변경
  clickInterest(event) {
    event.stopPropagation();
    let data = { hobbyClassId: this.item.id, interest: this.item.interest };
    this.setInterestClass(data);
    this.item.interest = !this.item.interest;
  },
 ```
 하트를 누르면 관심지역 설정하는 코드다. vue method에서 저걸 실행하면 Vuex의 action이 실행되는데 원래 코드가
 
 ```javscript
 // 관심 클래스 등록여부 변경
    async setInterestClass({ rootGetters, dispatch }, data) {
      // 로그인 했을 때만 변경 가능
      if (rootGetters.token && rootGetters.token != "") {
       await axios
        .post(SERVER.URL + SERVER.ROUTES.setInterestClass, data, {
          headers: rootGetters.authorization,
        })
        .then((res) => {
          dispatch("fetchClassList");
        })
        .catch((err) => {
          console.log(err);
        });
    } else {
      Swal.fire({
        text: "로그인 후 이용해주세요",
        showConfirmButton: false,
      });
      // 로그인 페이지로 보내기
      router.push({ name: "Login" });
    }
  },
 ```
 저렇다. 근데 axios로 서버와 통신하고 결과값을 return해줘야 할 일이 생겼다. 단순히 then 안에 `return "success"` 라고 하니 Pending이 결과값으로 오는 게 아닌가. 
 구글링 하다 보니 Axios 전체를 return Promise로 감싸줘야 되는 거였다
 -------------
 ```javascript
 this.setInterestClass(data).then((result) => console.log(result));
 ```
 ```javascript
  // 관심 클래스 등록여부 변경
    async setInterestClass({ rootGetters, dispatch }, data) {
      // 로그인 했을 때만 변경 가능
      if (rootGetters.token && rootGetters.token != "") {
        return new Promise((resolve, reject) => {
          axios
            .post(SERVER.URL + SERVER.ROUTES.setInterestClass, data, {
              headers: rootGetters.authorization,
            })
            .then((res) => {
              dispatch("fetchClassList");
              resolve("success");
            })
            .catch((err) => {
              console.log(err);
              reject("error");
            });
        });
      } else {
        Swal.fire({
          text: "로그인 후 이용해주세요",
          showConfirmButton: false,
        });
        // 로그인 페이지로 보내기
        router.push({ name: "Login" });
      }
    },
 
 ```

 
