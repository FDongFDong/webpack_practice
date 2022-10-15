# webpack_practice
웹팩을 학습하기 위한 공간

## 웹팩으로 해결하려는 문제
- 자바스크립트 변수 유효 범위
- 브라우저별 HTTP 요청 숫자의 제약
- 사용하지 않는 코드의 관리
- Dynamic Loading & Lazy Loading 미지원

## webpack 설정 및 빌드
[Part 1]()

1. webpack 설치
    ```
    npm i webpack webpack-cli -D
    ``` 
2. webpack.config.js 파일을 생성 후 옵션값들을 입력한다.
   [webpack.config.js]()
3. package.json 파일에 해당 내용을 추가한다.
   ```
   "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
    },
   ```
4. npm run build 명령어를 입력하여 빌드한다.
___

## webpack.config.js 옵션

주요 속성 4가지

1. entry : 빌드를 할 대상
  - 빌드하기 위해 필요한 최초의 진입점인 자바스크립트 파일 경로
2. output : 빌드 후 결과물에 대한 파일 경로
3. loader : 웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images 등)들을 변환할 수 있도록 도와주는 속성
4. plugin

- devtool: 'source-map'
  - 빌드한 결과물과 빌드되기전 결과물을 연결해주는 옵션, Chrome 개발자 도구에서 소스코드를 확인할 수 있다.

___

## Code Splitting

```javascript
var path = require('path');

module.exports = {
  mode: 'none', // production, development, none
  entry: './index.js',
  output: {
    filename: 'bundle.js', // [name].js, [chunkhash].js
    path: path.resolve(__dirname, 'dist'),
  },
  module: { // loader
    rules: [ // 하위에 들어가는 배열 인덱스 하나하나가 규칙이다.
      {
        test: /\.css$/, // 모든 css파일들을 대상으로
        use: ['style-loader', 'css-loader'], // 해당 파일들을 적용하겠다.
      },
    ],
  },
};

```

- use에 사용되는 파일들의 순서도 영향을 준다.
- style-loader : header안에 inline style로 들어갈 수 잇도록 한다..
- css-loader : css가 웹팩에 들어갈 수 있게한다.

___
## plugin 적용