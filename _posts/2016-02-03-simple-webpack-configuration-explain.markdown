---
published: false
title: Simple Webpack Configuration Explained
layout: post
tags: [webpack,javascript,reactjs]
categories: [webpack,javascript,reactjs]
---

I have been experimenting with reactjs the last couple of months and with that
I started using [webpack](https://webpack.github.io/) to handle building the projects.  I found the configuration for webpack a bit confusing when I first started out so I want to document how my basic configuration is setup and why.

A link to the full webpack configuration is I am using is [here](https://raw.githubusercontent.com/blaircamp/react-webpack-project/master/webpack.config.js)

#### Entry points

```js
  entry: [
    'webpack-dev-server/client?http://0.0.0.0:8081',
    'webpack/hot/only-dev-server',
    './index.jsx'
  ]

```
#### What is this?

* First Entry
```
webpack-dev-server/client?http://0.0.0.0:8081 //WebpackDevServer host and port
```


* Second Entry
```
webpack/hot/only-dev-server //"only" prevents reload on syntax errors
```

* Third Entry
```
./index.jsx //App entry point
```

#### Output
```js
  output: {
          path: path.join(__dirname, 'public'),
          filename: 'bundle.js'
  }
```
#### What is this?
This tells webpack where to put the bundled JavaScript file it has created. The path setting points to the directory where it should be saved, and the filename is what it should be called


#### Resolving Extensions
```js
  resolve: {
        extensions: ['', '.js', '.jsx','.css']
  },
```
#### What is this?
The resolve.extensions setting tells webpack what file extensions it should search for while resolving modules.  In the example below, if you want to import a local stylesheet for a reactjs component you will need to have the .css extension in the extension array.

```js
  import style from './style'
```



#### Modules
```js
  module: {
      loaders: [
          {
              test: /\.jsx?$/,
              exclude: /(node_modules|bower_components)/,
              loaders: ['react-hot', 'babel'],
          },

          {
              test: /\.css$/,
              loader: 'style-loader!css-loader?modules'
          }
      ]
  }
```
#### What is this?
###### JSX loader
###### CSS Loader

#### Dev Server Configuration
```js
  devServer: {
        contentBase: "./public",
        noInfo: true, //  --no-info option
        hot: true,
        inline: true
  },
```
#### What is this?

Changes For production

ExtractTextPlugin

#### All Together Now (Development)
```js
var webpack = require('webpack');
var path = require('path');

module.exports = {
    entry: [
    'webpack-dev-server/client?http://0.0.0.0:8081', // WebpackDevServer host and port
    'webpack/hot/only-dev-server',
    './index.jsx' // Your appʼs entry point
  ],
    devtool: process.env.WEBPACK_DEVTOOL || 'source-map',
    output: {
        path: path.join(__dirname, 'public'),
        filename: 'bundle.js'
    },
    resolve: {
        extensions: ['', '.js', '.jsx','.css']
    },
    module: {
        loaders: [
            {
                test: /\.jsx?$/,
                exclude: /(node_modules|bower_components)/,
                loaders: ['react-hot', 'babel'],
            },

            {
                test: /\.css$/,
                loader: 'style-loader!css-loader?modules'
            }
        ]
    },
    devServer: {
        contentBase: "./public",
        noInfo: true, //  --no-info option
        hot: true,
        inline: true
    },
    plugins: [
      new webpack.NoErrorsPlugin()
    ]
};
```
