---
title: Plugins
sort: 3
contributors:
  - TheLarkInn
---

**Plugins** are the [backbone](https://github.com/webpack/tapable) of webpack. webpack itself is built on the **same plugin system** that you use in your webpack configuration!!

They also serve the purpose of doing **anything else** that a [loader](/concepts/loaders) cannot do.

## Anatomy

A webpack **plugin** is a JavaScript object that has an `apply` property. This `apply` property is called by the webpack compiler, giving access to the **entire** compilation lifecycle.


**ConsoleLogOnBuildWebpackPlugin.js**

```javascript
function ConsoleLogOnBuildWebpackPlugin() {

};

ConsoleLogOnBuildWebpackPlugin.prototype.apply = function(compiler) {
  compiler.plugin('run', function(compiler, callback) {
    console.log("The webpack build process is starting!!!");

    callback();
  });
};
```

## Usage

Since **plugins** can take arguments/options, you must pass a `new` instance to the `plugins` property in your webpack configuration.

Depending on how you are using webpack, there are multiple ways to use plugins.

### Configuration

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: './dist'
  },
  module: {
    loaders: [
      {
        test: /\.(js|jsx)$/,
        loader: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

### Node API

**some-node-script.js**

```javascript
  const webpack = require('webpack'); //to access webpack runtime
  const configuration = require('./webpack.config.js');

  let compiler = webpack(configuration);
  compiler.apply(new webpack.ProgressPlugin());

  compiler.run(function(err, stats) {
    // ...
  });
```

T> Did you know: The example seen above is extremely similar to the [webpack runtime itself!](https://github.com/webpack/webpack/blob/master/bin/webpack.js#L290-L292) There are lots of great usage examples hiding in the [webpack source code](https://github.com/webpack/webpack) that you can apply to your own configurations and scripts!
