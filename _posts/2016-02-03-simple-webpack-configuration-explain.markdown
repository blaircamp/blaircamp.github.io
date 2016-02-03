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

A link to the base reactjs project I am using is [Here](https://github.com/blaircamp/react-webpack-project)

I have two webpack configurations because I wanted hot reloading to work with
my style sheets. The difference is that the production one extracts the css
into its own file for publishing and the other one does not.


```js


```
