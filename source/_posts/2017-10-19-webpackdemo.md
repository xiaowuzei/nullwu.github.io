---
title: webpackdemo
date: 2017-10-19 08:37:26
tags: [webpack]
---
## 基本用法
a.html

```
...
    <script src="index.js"></script>
..
```
people.js

```
module.exports='Hello EveryBody'
```

app.js

```
alert(require('./people')) //路径必须./即使是当前目录
```
执行命令`webpack app.js index.js`即生成index.js,
打开index.html弹出`'Hello EveryBody'`

执行命令的时候 --watch 即可监听，app.js有变化自动打包

## 运用第三方库
`cnpm install style-loader css-loader --save`
index.html
```
<script src='dist/index.js'></script>
```
style.css
```
body{
    background:red
}
```
app.js

`require("!style-loader!css-loader!./style.css")`

## 配置文件
webpack.config.js

```javascript
module.exports={
    // 入口文件
    entry:'./app.js',
    // 出口文件
    output:{
        path:__dirname+'/dist',
        filename:'index.js'
    },
    // 需要依赖的插件或者是装载器
    module:{
        loaders:[
            {test: /\.css$/,loader:"style-loader!css-loader"}
        ]
    }
}

```
app.js

```
require('./style.css')
```
`webpack`即可
webpack    开发环境下编译（打包）
webpack -p 生产环境下编译（压缩）
webpack -w 监听文件改动（自动编译）
webpack -d 开启(生成)source map
webpack -wdp 
## 自定义配置文件
webpack --config config.js
## package.json

```
  "scripts": {
    "build":"webpack"
  },
```
即可用 `npm run build`来代替执行`webpack`

## 搭建服务器运行
`cnpm install webpack-dev-server -g --save-dev`

package.json

加入start字段
```
  "scripts": {
    "start": "webpack-dev-server --entry ./app.js --output-filename ./dist/index.js",
    "build":"webpack"
  },
```
npm start 自动开启8080端口，自动监听无需刷新。这样就不需要执行webpack命令，或者说不需要在webpack.config.js中配置入口和出口，但是指定的加载器模块仍然要写上。
## es6
cnpm install babel-core babel-loader babel-preset-es2015 --save-dev

```javascript
module.exports = {

    // 需要依赖的插件或者是装载器
    module: {
        loaders: [{
                test: /\.css$/,
                loader: "style-loader!css-loader"
            },
            {
                test: /\.js$/,
                loader: "babel-loader",
                exclude: /node_modules/,
                query: {
                    presets: ["es2015"]
                }
            }

        ]
    }
}
```

```javascript
//另一种方式，去掉query,建立.babelrc
{
    "presets":['es2015']
}
```

## Demo2
```javascript
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const CleanWebpackPlugin = require('clean-webpack-plugin')
const ExtractTextPlugin = require('extract-text-webpack-plugin')
const webpack=require('webpack')
module.exports = {
    entry: {
        vendor:['jquery','./src/js/common.js'],
        // 多页面有几个页面就有几个入口,单页面只有一个入口
        index: './src/js/index.js',
        cart: './src/js/cart.js'
    },
    output: {
        path: path.join(__dirname, './dist'), //出口必须是绝对路径
        filename: 'js/[name].js',
        publicPath: ''
    },
    module: {
        rules: [{
            test: /\.css$/,
            include: path.join(__dirname, 'src'),
            exclude: /node_modules/,
            // 这种方法是抽取成单独的css文件
            // use: ExtractTextPlugin.extract({
            //     fallback: 'style-loader',
            //     use: 'css-loader'
            // })
            // 这种方法是行内
            loader:'style-loader!css-loader'
        },{
            test: /\.js$/,
            include: path.join(__dirname, 'src'),
            exclude: /node_modules/,
            loader:'babel-loader'
        }]
    },
    plugins: [
        // 打包后清除之前的文件
        new CleanWebpackPlugin(['./dist'], {
            root: path.join(__dirname, ''),
            verbose: true,
            dry: false
        }),
        // 单页面配置几个，多页面有几个页面new几个
        new HtmlWebpackPlugin({
            filename: 'cart.html',
            template: './src/cart.html',
            chunks: ['cart','vendor']
        }),
        new HtmlWebpackPlugin({
            filename: 'index.html',
            template: './src/index.html',
            chunks: ['index','vendor'], //指定引用的块，指定以后只引入index相关的css,js，不写这个打包后的htmml引入所有的index,css等。
            minify:{
                removeComments:true,
                collapseWhitespace:true
            }
        }),
        new webpack.ProvidePlugin({
          $:'jquery',
          jQuery:'jquery',
          'window.jQuery':'jquery'
        }),
         new ExtractTextPlugin('[name].css'),
         new webpack.optimize.CommonsChunkPlugin({
            name:'vendor',
            chunks:['index','cart','vendor'],
            minChunks:3
         }),
        //  压缩js
         new webpack.optimize.UglifyJsPlugin({
             compress:{
                 warnings:true
             }
         })

    ],
    // devtool: '#source-map'
}
```

### es6
- `npm install --save babel-core babel-loader babel-preset-env`
- 在配置文件同级目录下建一个.babelrc,写入 `{  "presets": ["env" ]}`
- 
```javascript
        {
            test: /\.js$/,
            include: path.join(__dirname, 'src'),
            exclude: /node_modules/,
            loader:'babel-loader'
        }
```