# Getting Started with React (개발환경세팅)

* NPM - 패키지 매니저
* Webpack - 모듈 번들러
* ES6 - 자바스크립트 표준버전
* Babel - 자바스크립트 컴파일러
* React - UI개발 자바스크립트 라이브러리

- [React cheat sheet](https://github.com/hohoya33/react-basic/blob/master/%5B01%5D%20Setting/react-cheat-sheet.pdf)
- [크롬 확장프로그램 react-developer-tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)


### node / npm
- [nodejs](https://nodejs.org/ko/)

**프로젝트 시작 (package.json 파일생성)**
```bash
$ npm init    (enter skip)
```
```bash
$ mkdir [folderName] && cd [folderName]
$ npm init -y        ( 생략 )
$ npm i jquery       ( i  === install )
$ npm i -S jquery    ( -S === --save )
$ npm i -D jquery    ( -D === --save-dev )
$ npm un jquery      ( un === uninstall )
```

## Getting Started Webpack (설치, 설정)
```bash
$ mkdir react-basic
$ cd react-basic
$ npm init -y

//webpack 설치
$ npm i webpack --save-dev

//webpack.config.js 생성
$ touch webpack.config.js
```

```js
//------ webpack.config.js ------
var path = require('path');
 
module.exports = {
    entry: {
        index: './src/index.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].bundle.js'
    }
}
```

```js
//------ package.json ------
"scripts": {
    "dev": "webpack -d --watch",
    "prod": "webpack -p"
}
```
### 디렉터리 구조
```
├── node_modules
├── src
│   ├── app.scss
│   ├── index.js
│   └── index.html
├── package.json
├── webpack.config.js
└── .babelrc

```



### HTML Webpack Plugin
```bash
$ npm i html-webpack-plugin --save-dev
```

```js
//------ webpack.config.js ------
var HtmlWebpackPlugin = require('html-webpack-plugin');
var path = require('path');

module.exports = {
    // ...
    plugins: [
        new HtmlWebpackPlugin({
            title: 'Project demo',
            // minify: {
            //     collapseWhitespace: true
            // },
            hash: true,
            //excludeChunks: ['contact'],
            template: './src/index.html'
        })
    ]
}
```

```js
//----- index.html -----
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title><%= htmlWebpackPlugin.options.title %></title>
</head>
<body>
	<div id="root"></div>
</body>
</html>
```

### Style, CSS and Sass loaders
- css-loader: css 파일 로드
- style-loader: html 브라우저에서 스타일 적용

```bash
//css-loader, style-loader
$ npm i css-loader style-loader --save-dev

//Sass-loader
$ npm i sass-loader node-sass --save-dev

//Scss 순수 css 변환
$ npm i extract-text-webpack-plugin --save-dev
```
```js
//------ webpack.config.js ------
var ExtractTextPlugin = require("extract-text-webpack-plugin");
var path = require('path');

module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: ExtractTextPlugin.extract({
                    fallback: 'style-loader',
                    use: ['css-loader','sass-loader'],
                    publicPath: '/dist'
                })
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin({
            filename: 'app.css',
            disable: false,
            allChunks: true
        })
    ]
}
```

### Setting up React and Babel
```bash
//React 설치
$ npm i react react-dom --save-dev

//Babel 설치
$ npm i babel-core babel-loader babel-preset-es2015 babel-preset-react --save-dev

//.babelrc 파일생성
$ touch .babelrc
```

```js
//------ .babelrc ------
{
    "presets":[
        "es2015", "react"
    ]
}
```

```js
//------ webpack.config.js ------
module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: [{
                    loader: 'babel-loader',
                    options: {presets: ['es2015']}
                }]
            }
        ]
    }
}
```

### Webpack Dev Server
```bash
$ npm i webpack-dev-server --save-dev
```

```js
//------ webpack.config.js ------
module.exports = {
    // ...
    devServer: {
        contentBase: path.join(__dirname, "dist"),
        compress: true,
        port: 8080,
        stats: "errors-only",
        open: true
    }
}
```
```js
//------ package.json ------
"scripts": {
    "dev": "webpack-dev-server",
    "prod": "webpack -p"
}
```

```bash
$ npm run dev
```