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



### RimRaf
* dist 폴더 파일 삭제
rimraf 명령어를 통해 삭제 가능합니다. 
먼저 rimraf 모듈을 설치합니다.
rimraf 명령을 통해 원하는 폴더 경로를 입력해서 삭제합니다.

```bash
$ npm i rimraf --save-dev
```

```js
//------ package.json ------
 "scripts": {
	"dev": "webpack-dev-server",
	"prod": "npm run clean && webpack -p",
	"clean": "rimraf ./dist/*"
}
```

### How to load images with Webpack 2
```bash
$ npm i file-loader --save-dev
$ npm i image-webpack-loader --save-dev
```

```js
//------ webpack.config.js ------
module: {
    rules: [
        { 
            test: /\.(jpe?g|png|gif|svg)$/i, 
            use: [
                    'file-loader?name=images/[name].[ext]',
                    'image-webpack-loader' 
            ]
        }
    ],
}
```


## Hot Module Replacement - CSS

```js
//------ webpack.config.js ------
var webpack = require('webpack');

module.exports = {
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: ['style-loader', 'css-loader?sourceMap', 'sass-loader']
            }
        ],
    },
    devServer: {
        contentBase: path.join(__dirname, "dist"),
        compress: true,
        port: 8080,
        stats: "errors-only",
        hot: true,
        open: true
    },
    plugins: [
        new ExtractTextPlugin({
            filename: 'app.css',
            disable: true,
            allChunks: true
        }),
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NamedModulesPlugin()
    ]
}
```


### Production vs Development Environment

```js
//------ package.json ------
"scripts": {
	"dev": "webpack-dev-server",
	"prod": "npm run clean && NODE_ENV=production webpack -p",
	"clean": "rimraf ./dist/*"
}
```

```js
//------ webpack.config.js ------
var isProd = process.env.NODE_ENV === 'production'; //true or false

var cssDev = ['style-loader', 'css-loader?sourceMap', 'sass-loader'];
var cssProd = ExtractTextPlugin.extract({
    fallback: 'style-loader',
    use: ['css-loader','sass-loader'],
    publicPath: '/dist'
});

var cssConfig = isProd ? cssProd : cssDev;

module.exports = {
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: cssConfig
            }
        ],
    },
    plugins: [
        new ExtractTextPlugin({
            filename: 'app.css',
            disable: !isProd,
            allChunks: true
        })
    ]
}
```


### Bootstrap Load
```bash
$ npm i bootstrap-loader --save-dev
$ npm i bootstrap-sass --save-dev
$ npm i resolve-url-loader url-loader --save-dev
```
* create .bootstraprc file in the root folder
https://raw.githubusercontent.com/shakacode/bootstrap-loader/master/.bootstraprc-3-default

* create webpack.bootstrap.config.js
https://raw.githubusercontent.com/shakacode/bootstrap-loader/master/examples/basic/webpack.bootstrap.config.js

```js
//------ webpack.config.js ------
var isProd = process.env.NODE_ENV === 'production'; //true or false

var bootstrapEntryPoints = require('./webpack.bootstrap.config');
var bootstrapConfig = isProd ? bootstrapEntryPoints.prod : bootstrapEntryPoints.dev;

module.exports = {
    entry: {
        index: './src/index.js',
        bootstrap: bootstrapConfig
    }
}
```

* Icon fonts
npm run prod 시
dist 폴더에 fonts 폴더 생성 후 파일을 묶음
?name=fonts/[name].[ext]

```js
//------ webpack.config.js ------
module.exports = {
    module: {
        rules: [
            { test: /\.(woff2?|svg)$/, loader: 'url-loader?limit=10000&name=fonts/[name].[ext]' },
            { test: /\.(ttf|eot)$/, loader: 'file-loader?name=fonts/[name].[ext]' },
        ],
    },
    plugins: [
        new ExtractTextPlugin({
            filename: '/css/[name].css',
            disable: !isProd,
            allChunks: true
        }),
    ]
}
```

* jQuery
```bash
$ npm i imports-loader jquery --save-dev
```

```js
//------ webpack.config.js ------
module.exports = {
    module: {
        rules: [
            { test:/bootstrap-sass[\/\\]assets[\/\\]javascripts[\/\\]/, loader: 'imports-loader?jQuery=jquery' }
        ],
    }
}
```

* Bootstrap Customizations

./node_modules/bootstrap-sass/assets/stylesheets/bootstrap/_variables.scss
스타일 커스터마이징

```js
//------ .bootstraprc ------
# bootstrapCustomizations: ./path/to/bootstrap/customizations.scss
//아래 경로로 변경 후 src폴더에 bootstrap폴더 생성 후 customizations.scss 파일 생성
# bootstrapCustomizations: ./src/bootstrap/customizations.scss
```

```css
<!-- app.scss -->
@import './bootstrap/customizations.scss';

body {
    h1 {
        color: $brand-success;
    }
}
```






