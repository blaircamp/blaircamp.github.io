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

### Entry points

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
This adds the node_modules/webpack-dev/client/index.js script into the bundle.  This will connect to the webpack-dev-server and reload the application when new changes are detected. The URL is pointing to the webback-dev-server.
```
//command line used for the dev server
webpack-dev-server --progress --profile --colors --hot --port 8081
```

* Second Entry
```
webpack/hot/only-dev-server //"only" prevents reload on syntax errors
```
This adds the node_modules/webpack/hot/only-dev-server.js script into the bundle.  This will determine if the change that has been made can be applied as a hot update without refreshing the page and losing the current state of the application.   More information on hot module replacement in webpack can be found [here](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html)

* Third Entry
```
./index.jsx //App entry point
```
This is the main entry point into the application, everything else is there to facilitate reloading the page when webpack rebuilds the bundle after a change has been made.


### Output
```js
  output: {
          path: path.join(__dirname, 'public'),
          filename: 'bundle.js'
  }
```
#### What is this?
This tells webpack where to put the bundled JavaScript file it has created. The path setting points to the directory where it should be saved, and the filename is what it should be called


### Resolving Extensions
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



### Modules
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


### Dev Server Configuration
```js
  devServer: {
        contentBase: "./public",
        noInfo: true, //  --no-info option
        inline: true
  },
```
#### What is this?
This setting is used to configure the webpack-dev server that the configuration
is passed into.

The ** contentBase ** setting tells the server to serve the files from this directory path.

The ** noInfo ** setting is used to suppress some information.

The ** inline ** setting is used to add the webpack-dev-server runtime into the
webpack bundle

This configuration is the same as running this on the command line web starting
the webpack-dev server.
```
webpack-dev-server --context-base './public ' --no-info  --inline
```

More information on these swtiches and configuration can be found [here](https://webpack.github.io/docs/webpack-dev-server.html#webpack-dev-server-cli)

### All Together Now
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
        inline: true
    },
    plugins: [
      new webpack.NoErrorsPlugin()
    ]
};
```
