# webpack_practice
웹팩을 학습하기 위한 공간

## 웹팩으로 해결하려는 문제
- 자바스크립트 변수 유효 범위
- 브라우저별 HTTP 요청 숫자의 제약
- 사용하지 않는 코드의 관리
- Dynamic Loading & Lazy Loading 미지원

## webpack 설정 및 빌드
> [Part 1](https://github.com/FDongFDong/webpack_practice/tree/main/part1)

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
3. loader(module) : 웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images 등)들을 변환할 수 있도록 도와주는 속성
4. plugin : 추가적인 기능을 제공하는 속성

- devtool: 'source-map'
  - 빌드한 결과물과 빌드되기전 결과물을 연결해주는 옵션, Chrome 개발자 도구에서 소스코드를 확인할 수 있다.

___

## Loader
[webpack/loader](https://webpack.js.org/loaders/)

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
> [source code: part3](https://github.com/FdongFdong/webpack_practice/tree/main/part3)
[webpack/plugins](https://webpack.js.org/plugins/)

- 웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성
- 결과물의 형태를 바꾸거나 부가적인 역할을 한다.
  - css파일이 분리되어 dist/main.css로 만들어지는 것을 확인할 수 있다.
- 플러그인의 배열에는 생성자 함수로 생성한 객체 인스턴스만 추가될 수 있다.


```javascript
var path = require('path');
var MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'none',
  entry: './index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: MiniCssExtractPlugin.loader }, 'css-loader'
        ],
      },
    ],
  },
  plugins: [new MiniCssExtractPlugin()], // 객체 인스턴스
};

```
___


## Webpack Dev Server

- 코드 수정 시 즉각 반영되어 빌드된 파일이 수정되도록 하기 위함
  1. 코드 수정
  2. build 진행
- 매번 진행함에 있어 불편함을 해소하기 위함
- 자동으로 웹팩 빌드 후 브라우저 새로고침까지 진행한다.


1. 설치
```shell
npm i webpack webpack-cli webpack-dev-server html-webpack-plugin -D
```
2. package.json 수정
```
  "scripts": {
    "dev": "webpack-dev-server"
  },
```

3. webpack.config.js 수정
```javascript
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin'); 

module.exports = {
  mode: 'none',
  entry: './index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  devServer: {  // 카멜케이스로 진행한다.
    port: 9000, // localhost:9000에 Server를 연다.
    // overlay: false 등 다양한 옵션을 지원한다. 
  },
  plugins: [
    new HtmlWebpackPlugin({ 
      // index.html 템플릿을 기반으로 빌드 결과물을 추가해줌
      template: 'index.html',
    }),
  ],
};
 ```
4. localhost:9000 이동하여 확인
  


- 코드의 변경사항이 발생하면 바로 재빌드하고 웹페이지를 새로고침한다.
- 메모리 상으로만 빌드 결과를 올려 놓기 때문에 파일시스템에서 보이지 않는다.
- 해당 프로젝트가 완성되면 package.json에 'build': 'webpack'를 추가하여 빌드한다.

HtmlWebpackPlugin으로 HTML 결과물을 만들어주고 그안에 내용들(templage)을 포함해서 webpack에 들어가도록 한다.
