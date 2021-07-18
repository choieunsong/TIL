# WebPack이란?
자바스크립트 코드가 많이자며 하나의 파일로 관리하는 데 한계가 있다. 
webpack은 JavaScript에서 코드를 모듈로 나누어 관리하는 모듈 시스템을 지원한다. `모듈시스템` 외에도 로더 사용, 빠른 컴파일 속도 등 장점이 많다.


## 모듈의 정의와 모듈 사용
모듈을 작성하고 다음과 같이 module.exports 속성에 외부에 배포할 모듈을 전달해 모듈을 정의한다.
```javascript
module.exports = {message: 'webpack'}
```
모듈을 사용할 때는 모듈을 로딩하는 파일의 require() 함수에 로딩할 모듈의 경로를 전달한다. 모듈을 정의하는 examplemodule.js 파일을 사용한다고 하면
```javascript
alert(require('./examplemodule').message)
```

<br>

**<q>웹팩의 주요 네 가지 개념</q>**
> 엔트리, 아웃풋, 로더, 플러그인

### 엔트리
웹팩에서 모든 것은 모듈이다. 자바스크립트, 스타일시트, 이미지 등 모든 것을 자바스크립트 모듈로 로딩해서 사용하도록 한다.  

![image](https://user-images.githubusercontent.com/24693833/126060066-89a34ba3-98ac-4bd1-a359-a2d2e2bc44bb.png)

엔트리는 각 의존성 그래프에서 시작점을 가리킨다. 웹팩은 엔트리를 통해서 필요한 모듈을 로딩하고 하나의 파일로 묶는다.  

webpack.config.js:
```javascript
module.exports = {
    entry: {
        main: "./src/main.js",
    },
}
```
우리가 사용할 html에서 사용할 자바스크립트 시작점은 src/main/js 코드다. `entry`키에 시작점 경로를 지정했다.

### 아웃풋
엔트리에 설정한 자바스크립트 파일을 시작으로 의존되어 있는 모듈을 하나로 묶을 것이다. 번들된 결과물을 처리할 위치는 `output`에 기록한다.  
webpack.config.js:
```javascript
module.exports = {
    output: {
        filename: "bundle.js",
        path: "./dist",
    },
}
```
dist 폴더의 bundle.js 파일로 결과를 저장할 것이다.html 파일에서는 번들링된 이 파일을 로딩하게끔 한다.


index.html:
```javascript
<body>
    <script src="./dist/bundle.js"></script>
</body>
```