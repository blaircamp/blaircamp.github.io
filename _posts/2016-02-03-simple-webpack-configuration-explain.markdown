---
published: false
title: Simple Webpack Configuration Explained
layout: post
tags: [webpack,javascript,reactjs]
categories: [webpack,javascript,reactjs]
---

I have been experimenting with reactjs the last couple of months and with that
I started using [webpack](https://webpack.github.io/) to build the projects
into a bundle.js.  I found the configuration for webpack a bit confusing so I
am going to run through how I ended up configuring webpack.

A link to the full webpack configuration is I am using is [Here](https://raw.githubusercontent.com/blaircamp/react-webpack-project/master/webpack.config.js)

I have two webpack configurations because I wanted hot reloading to work with
my style sheets. The difference is that the production one extracts the css
into its own file for publishing and the other one does not.


Entry points

```js
  entry: [
    'webpack-dev-server/client?http://0.0.0.0:8081',
    'webpack/hot/only-dev-server',
    './index.jsx'
  ]

```

Output
```js
  output: {
          path: path.join(__dirname, 'public'),
          filename: 'bundle.js'
  }
```

Resolving Extensions
```js
  resolve: {
        extensions: ['', '.js', '.jsx','.css']
  },
```

Modules
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

Dev Server Configuration
```js
  devServer: {
        contentBase: "./public",
        noInfo: true, //  --no-info option
        hot: true,
        inline: true
  },
```

Changes For production

ExtractTextPlugin

All Together Now
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
