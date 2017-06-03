# Getting Started with React (개발환경세팅)

1. NPM - 패키지 매니저
1. Webpack - 모듈 번들러
1. ES6 - 자바스크립트 표준버전
1. Babel - 자바스크립트 컴파일러
1. React - 자바스크립트 라이브러리

## node / npm

- [nodejs](https://nodejs.org/ko/)

**프로젝트 시작 (package.json 파일생성)**
```bash
npm init    (enter skip)
npm init -y (생략 한번에)
```

## package.json

- [모두 알지만 모두 모르는 package.json](http://programmingsummaries.tistory.com/385)

```bash
$ mkdir [folderName] && cd [folderName]
$ npm init -y
$ npm i jquery       ( i  === install )
$ npm i -S jquery    ( -S === --save )
$ npm i -D jquery    ( -D === --save-dev )
$ npm un jquery      ( un === uninstall )
```

## Webpack 설치 및 설정
```bash
npm i webpack --save-dev

touch webpack.config.js
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
│   ├── App.js
│   ├── index.js
│   └── index.html
├── package.json
├── webpack.config.js
└── .babelrc

```



### HTML Webpack Plugin
```bash
npm i html-webpack-plugin --save-dev
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

### Style, CSS and Sass loaders
- css-loader: css파일 로드
- style-loader: html에 브라우저에서 스타일 적용

```bash
//css-loader, style-loader
npm i css-loader style-loader --save-dev

//Sass-loader
npm i sass-loader node-sass --save-dev

//Scss 순수 css 변환
npm i extract-text-webpack-plugin --save-dev
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


### Webpack Dev Server
```bash
npm i webpack-dev-server --save-dev
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

### Setting up React and Babel
```bash
//React
npm i react react-dom --save-dev

//Babel
npm i babel-core babel-loader babel-preset-es2015 babel-preset-react --save-dev

touch .babelrc
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