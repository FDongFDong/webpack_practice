# webpack_practice
웹팩을 학습하기 위한 공간

## 웹팩으로 해결하려는 문제
- 자바스크립트 변수 유효 범위
- 브라우저별 HTTP 요청 숫자의 제약
- 사용하지 않는 코드의 관리
- Dynamic Loading & Lazy Loading 미지원

## webpack 설정 및 빌드
1. webpack.config.js 파일을 생성 후 옵션값들을 입력한다.
   [webpack.config.js]()
2. package.json 파일에 해당 내용을 추가한다.
   ```
   "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
    },
   ```
3. npm run build 명령어를 입력하여 빌드한다.