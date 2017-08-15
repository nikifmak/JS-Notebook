# Webpack Understanding

```javascript
var path = require('path');
var webpack = require('webpack');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    publicPath: path.resolve(__dirname,'/dist'),
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      { test: /\.html$/, loader: "html" },
      { test: /\.css$/, loader: "style!css" }
    ]
  },
  watch: true,
  plugins: [
   new webpack.HotModuleReplacementPlugin()
  ],
  devServer: {
    hot: true,
    contentBase: path.resolve(__dirname,'dist'),
    historyApiFallback: true
  }

}
```



## File System

./

+— _dist

​       +— index.html

​       +— ha.js

+— _src

​       +— index.js



### devServer

>  contentBase: path.resolve(__dirname,'dist')

—> dev-server servers the **`/dist`**

which is correct becasuse when we start the server, it correcty shows the `index.html` 

### When we serve a static file from **`/dist`**

*ha.js* is correctly served

> ```html
>
> <!DOCTYPE html>
> <html>
>   <head>
>     <meta charset="utf-8">
>     <title>ReactJS with Webpack and ES6</title>
>   </head>
>   <body>
>     <script src="hah.js"></script>
>     <div class="application"></div>
>   </body>
> </html>
> ```

### When we serve the *bundle.js* though 

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>ReactJS with Webpack and ES6</title>
  </head>
  <body>
    <script src="bundle.js"></script>
    <div class="application"></div>
  </body>
</html>

```

We get the following error

>```http
>Request URL:http://localhost:8080/bundle.js
>Request Method:GET
>Status Code:404 Not Found
>```

Lets understand what is happening and what we wrongly setup on the webpack.config.js

### What works 

The first thing we should notice is that if we change the *index.html* to :

>```html
><!DOCTYPE html>
><html>
>  <head>
>    <meta charset="utf-8">
>    <title>ReactJS with Webpack and ES6</title>
>  </head>
>  <body>
>    <script src="dist/bundle.js"></script>
>    <script src="hah.js"></script>
>    <div class="application"></div>
>  </body>
></html>
>```

It works correctly.

So with this config it seems that the generated ***bundle.js*** is on virtual path ***dist/bundle.js*** and webpack-dev-server serves the ***index.html*** on root ***''/''*** 

### Webpack Output config

|                   |                                 |                                          |
| ----------------- | ------------------------------- | ---------------------------------------- |
| output.path:      | path.resolve(__dirname, 'dist') | The output directory as an **absolute** path, namely the folder path in our file system in which the generated file will be saved. |
| output.publicPath | path.resolve(__dirname,'/dist') | Where the *bundle.js* generated file which resides in the memory of the server will be avaible. |

> We change This is an important option when using on-demand-loading or loading external resources like images, files, etc. If an incorrect value is specified you'll receive 404 errors while loading these resources.
>
> This option specifies the **public URL** of the output directory when referenced in a browser. A relative URL is resolved relative to the HTML page (or `<base>` tag). Server-relative URLs, protocol-relative URLs or absolute URLs are also possible and sometimes required, i. e. when hosting assets on a CDN.
>
> The value of the option is prefixed to every URL created by the runtime or loaders. Because of this **the value of this option ends with /** in most cases.
>
> The default value is an empty string `""`.

So we change **output.publicPath** to **path.resolve(__dirname,'/')** OR to simply **'/'**. Now the generated on memory bundled file is available on root context path. So the *index.html*:

> ```html
> <!DOCTYPE html>
> <html>
>   <head>
>     <title>ReactJS with Webpack and ES6</title>
>   </head>
>   <body>
>     <script src="bundle.js"></script>
>     <div class="application"></div>
>   </body>
> </html>
> ```

Now can access **bundle.js** without any problems !

So the final *webpack.config.js* is:

> ```json
> var path = require('path');
> var webpack = require('webpack');
>
> module.exports = {
>   entry: './src/index.js',
>   output: {
>     filename: 'bundle.js',
>     publicPath: path.resolve(__dirname,'/'),
>     path: path.resolve(__dirname, 'dist')
>   },
>   module: {
>     loaders: [
>       {
>         test: /\.js$/,
>         loader: 'babel-loader',
>         exclude: /node_modules/
>       },
>       { test: /\.html$/, loader: "html" },
>       { test: /\.css$/, loader: "style!css" }
>     ]
>   },
>   watch: true,
>   plugins: [
>    new webpack.HotModuleReplacementPlugin()
>   ],
>   devServer: {
>     hot: true,
>     contentBase: path.resolve(__dirname,'dist'),
>     historyApiFallback: true
>   }
>
> }
> ```
