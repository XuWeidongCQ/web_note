# 一、简介

* webpack是一个前端资源构建工具，一个静态模块打包器
* 在webpack看来，所有的资源文件(js/css/img/less...)都会作为模块处理

## 1.1 为什么要使用

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200701160606616.png" alt="image-20200701160606616" style="zoom:67%;" />

* 在写项目的时候，我们会用到一些方便的东西，比如ES6的import语句(图中的index.js文件)，less语法等，但是这些浏览器是不能识别的
* 除非引用专门的工具进行转换，非常麻烦。
* 所以提出一个综合的构建工具，进行转换，你如webpack

## 1.2 使用方法

* 需要指定一个打包的入口 所有依赖的资源 ----> chunk ----> bundle

## 1.3 五个核心概念

### 1.3.1 Entry

* 指定打包的开始文件

### 1.3.2 Output

* webpack打包后的bundle存放在哪里，以及如何命名

### 1.3.3 Loader

* 帮助webpack处理一些非js的文件，webpack默认只能处理js文件和json文件
* loader是一个nodejs模块

### 1.3.4 Plugins

* 可以执行比Loader更广的任务，比如打包优化和压缩，定义环境变量等
* 插件是一个构造函数

### 1.3.5 Mode

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200701161821207.png" alt="image-20200701161821207" style="zoom:80%;" />

* 生产环境比开发环境多一个对js的压缩过程，

## 1.4 打包样式资源

```
npm i style-loader css-loader -D
npm i less-loader -D
-D 表示开发依赖
```

* 新建一个webpack的配置文件webpack.config.js，和src文件同级

* 当运行webpack指令的时候，会加载里面的配置

* 构建工具都是基于nodejs的，模块化采用commonjs,只有项目中使用的ES6模块

  ```js
  //这是commonjs写法，只能这样写
  
  //resolve是用来拼接绝对路径的方法
  const {resolve} = require('path') //这是nodejs的模块
  
  module.exports = {
      entry:'./src/index.js',
      output:{
          filename:'built.js',
          //__dirname nodejs的变量 代表当前文件的目录(其父文件夹所在的位置)绝对路径
          path: resolve(__dirname,'build')
      },
      //loader
      module:{
          rules:[
              {
                  //正则匹配满足条件的文件后缀
                  test:/\.css$/,
                  //从下到上执行
                  use:[
                      'style-loader', //创建style标签,将js中的样式资源插入进行，添加到head生效
                      'css-loader'//将css文件变成commonjs模块加载到js中，里面的内容是样式的字符串形式
                  ]
              },
              {
                  test:/\.less$/,
                  //从下到上执行
                  use:[
                      'style-loader', 
                      'css-loader',
                      'less-loader'//将less文件编译成css文件
                  ]
              },
          ]
      },
    //插件
      plugins:[
          
      ],
      //模式
      //mode:'production'
      mode:'development'
  }
  ```

## 1.5 打包HTML文件

```
npm i html-webpack-plugin -D
```

* 使用插件html-webpack-plugin

```js
const {resolve} = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin') //这是一个类构造函数

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'built.js',
        //__dirname nodejs的变量 代表当前文件的目录(其父文件夹所在的位置)绝对路径
        path: resolve(__dirname,'build')
    },
    module:{
        rules:[
            {}
        ]
    },
    plugins:[
        //功能：默认会在build文件夹中创建一个空的HTML文件，引入打包输出的所有的资源，比如js/css
        new HtmlWebpackPlugin({
            //如果本来就有一个index.html文件，就会进行复制，并自动引入其他打包的资源
            template:'./src/index.html'
        })
    ],
    mode:'development'
}
```

## 1.6 打包图片资源

```
npm i url-loader file-loader -D
npm i html-loader -D
```

* 处理那些在样式中使用的图片
* img标签中的图片

```css
#box {
    background-image:url('./vue.jpg')
}
```

```js
const {resolve} = require('path')

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'built.js',
        //__dirname nodejs的变量 代表当前文件的目录(其父文件夹所在的位置)绝对路径
        path: resolve(__dirname,'build')
    },
    module:{
        rules:[
            {
                test:/\.less$/,
                use:['style-loader', 'css-loader','less-loader']
            },
            {
                //这种不能处理img标签中的图片
                test:/\.(jpg|png|gif)/,
                loader:'url-loader', //只用一个loader，可以这样写
                options:{
                    //图片小于8Kb,就会被base64处理 limit的意思
                    //优点：减少请求数量(base64会减轻服务器压力)
                    //缺点：图片体积会更大(文件请求速度更慢)
                    limit:8 * 1024,
                    name:'[hash:10].[ext]' //名字只取前10位
                }
            },
            {
                test:/\.html$/,
                loader:'html-loader' //处理html文件中的img图片
            }
        ]
    },
    plugins:[
        //功能：默认会在build文件夹中创建一个空的HTML文件，引入打包输出的所有的资源，比如js/css
        new HtmlWebpackPlugin({
            //如果本来就有一个index.html文件，就会进行复制，并自动引入其他打包的资源
            template:'./src/index.html'
        })
    ],
    mode:'development'
}
```

## 1.7 打包其他资源

* 字体，图标等

```
使用file-loader
```



```js
const {resolve} = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin') //这是一个类构造函数

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'built.js',
        //__dirname nodejs的变量 代表当前文件的目录(其父文件夹所在的位置)绝对路径
        path: resolve(__dirname,'build')
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader']
            },
            {
                exclude:/\.(css|js|html)$/, // 排除这些资源
                use:['file-loader']
            }
        ]
    },
    plugins:[
        //功能：默认会在build文件夹中创建一个空的HTML文件，引入打包输出的所有的资源，比如js/css
        new HtmlWebpackPlugin({
            //如果本来就有一个index.html文件，就会进行复制，并自动引入其他打包的资源
            template:'./src/index.html'
        })
    ],
    mode:'development'
}
```

# 二、devServer

* 使得能够进行热拔插
* 能够实现自动打包，自动刷新浏览器

```js
const {resolve} = require('path')
module.exports = {
    ...
    //只会在内存中编译打包，不会输出任何打包后的文件
    devServe:{
        contentBase:resolve(__dirname,'build'),
        //启动gzip压缩
        compress:true
        //端口号
        port:3000
        //自动打开浏览器
        open:true
    }
}
```

# 三、开发环境的基本配置

* 开发项目的时候用

```js
const {resolve} = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'js/built.js', //将js文件输出到js目录
        path:resolve(__dirname,'build')
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader']
            },
            {
                test:/\.less$/,
                use:['style-loader', 'css-loader','less-loader']
            },
            {
                test:/\.(jpg|png|gif)/,
                loader:'url-loader', 
                options:{
                    limit:8 * 1024,
                    name:'[hash:10].[ext]' //名字只取前10位
                    esModule:false, 
                    outputPath:'imgs' //将图片输出到imgs目录中
                }
            },
            {
                test:/\.html$/,
                loader:'html-loader' //处理html文件中的img图片
            },
            {
                exclude:/\.(css|js|html|less|jpg|png|gif)$/, // 排除这些资源
                use:['file-loader'],
                options:{
                    name:'[hash:10].[ext]' //名字只取前10位
                    outputPath:'media'
                }
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        })
    ],
    devServe:{
        contentBase:resolve(__dirname,'build'),
        compress:true
        port:3000
        open:true
    }
    mode:'development'
}
```

# 四、生产环境的搭建

* 生产环境打包后的文件是可以直接放到线上的
* 需要多做一些压缩和兼容不同浏览器的处理

## 4.1 css处理

### 4.1.1 将css单独抽取在一个文件夹中

* 使用插件

```
npm i mini-css-extract-plugin -D
```

```js
const MiniCssExtractPlugin = import('mini-css-extract-plugin')

//修改webpack的配置如下
module.exports = {
    ...
    module:{
       rules:[
           {
               test:/\.css$/,
               use:[
                   //取代style-loader 可以提取css为单独文件 这样的处理css中的图片需要设置publicPath属性
                   MiniCssEctractPlugin.loader,
                   'css-loader'
               ]
           }
       ] 
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        }),
        new MiniCssExtractPlugin({
            //对单独的css文件从命名
            filename:'css/built.css'
        })
    ],
}
```

### 4.1.2 css的兼容性处理

* 使用loader

```
npm i postcss-loader postcss-preset-env -D
```

```js
const MiniCssExtractPlugin = import('mini-css-extract-plugin')

//修改webpack的配置如下
module.exports = {
    ...
    module:{
       rules:[
           {
               test:/\.css$/,
               use:[
                   //取代style-loader 可以提取css为单独文件 这样的处理css中的图片需要设置publicPath属性
                   MiniCssEctractPlugin.loader,
                   'css-loader',
                   //如果单独修改某一个loader的配置需要这样写
                   //postcss-preset-env可以找到package.json中browserlist里面的配置，通过配置加载指定的css兼容性样式
                   {
                       loader:'postcss-loader',
                       options:{
                           ident:'postcss',
                           plugins:() => {
                               require('postcss-preset-env')()
                           }
                       }
                   },
               ]
           }
       ] 
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        }),
        new MiniCssExtractPlugin({
            //对单独的css文件从命名
            filename:'css/built.css'
        })
    ],
}
```

### 4.1.3 压缩css

* 使用插件

```
npm i optimize-css-assets-webpack-plugin -D
```

```js
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')

module.exports = {
    ...
   plugins:[
        ...,
       new OptimizeCssAssetsWebpackPlugin()
    ],
}
```

## 4.2 js处理

### 4.2.1 js语法检查

* eslint eslint-loader

```js
const MiniCssExtractPlugin = import('mini-css-extract-plugin')

//修改webpack的配置如下
module.exports = {
    ...
    module:{
       rules:[
           {
               test:/\.js$/,
               exclude:/node_modules/ //排除第三方库的js
               loader:'eslint-loader',
               options:{
               		fix:true //不符合规则自动修复
                }
           }
       ] 
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        }),
    ],
}
```

### 4.2.2 js兼容性处理

* 有些浏览器不支持es6，需要处理
* 下载三个库

```
npm i babel-loader @babel/preset-env @babel-core -D

npm i  @babel/polyfill  -D 执行全部的js兼容性处理 但是这样打包体积太大

npm i  core-js  -D 按需做兼容性处理
```

```js
module.exports = {
    ...
    module:{
       rules:[
           {
               test:/\.js$/,
               exclude:/node_modules/ //排除要转换的
               loader:'babel-loader',
               options:{
                   //预设：指示babel做什么样的兼容性处理
                   presets:[
               		'@babel/preset-env'，
               		{
               			useBuiltIns:'usage', //按需加载
               			corejs:{version:3},
           				targets:{chrome:'60'} //指定兼容性做到浏览器的哪个版本
           			}
                 ]
               }
           }
       ] 
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        }),
    ],
}
```

### 4.2.3 压缩html和js

```js

module.exports = {
    ...
    module:{
       rules:[
           {
               test:/\.js$/,
               exclude:/node_modules/ //排除第三方库的js
               loader:'eslint-loader',
               options:{
               		fix:true //不符合规则自动修复
                }
           }
       ] 
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html',
            minify:{ //启用html压缩
                collapseWhitespace:true,//移除空格
                removeComments:true//移除注释
            }
        }),
    ],
    mode:'production' //调整为生产环境自动启动js压缩
}
```

# 五、性能优化++

## 5.1 开发环境性能优化

* 优化打包构建速度
* 优化代码调试功能

### 5.1.1 优化打包速度

* 传统的，如果有一个模块被修改，那么所有的模块都会被重新执行一遍，在模块很多的时候很慢

* 改进：使用HMR(hot module replacement) 使得一个模块发生变化，只会打包准备一个模块，极大提高打包速度，设置如下：

  ```js
  module.exports = {
      ...
      //只会在内存中编译打包，不会输出任何打包后的文件
      devServe:{
          contentBase:resolve(__dirname,'build'),
          compress:true
          port:3000
          open:true
          hot:true //打开热拔插功能 热模块替换
      }
  }
  ```

* 样式文件可以使用HMR功能，style-loader内部实现了
* js文件 默认没有HMR功能
* html文件 默认没有HMR功能(不用做HMR功能)

### 5.1.2 优化代码调试功能

* source-map 可以提供源代码到构建后的代码映射技术 进行错误追踪

```js
module.exports = {
    ...
    //只会在内存中编译打包，不会输出任何打包后的文件
    devServe:{
        contentBase:resolve(__dirname,'build'),
        compress:true
        port:3000
        open:true
        hot:true //打开热拔插功能 热模块替换
    },
    devtool:'source-map' //开启
}

//可以取的值
[inline-|hidden-|eval-]       [nosource-]         [cheap-[module-]]     source-map


source-map //外联 功能:这个可以把错误定位到源代码准确位置
[可以隐藏原始代码]
hidden-source-map //外部的source-map 会产生单独的文件 功能:能提示错误信息，不能定位到源代码错误的位置，只能定位到构建后的
nosources-source-map //外部文件 功能:能提示错误信息 找不到错误位置
cheap-source-map //外部 功能:能提示错误信息，能定位到源代码错误的位置 但是只能精确到行 没有列
cheap-module-source-map//外部 功能:能提示错误信息，能定位到源代码错误的位置



eval-sourec-map //内联 每一个js文件都生成一个内联 功能:能提示错误信息，能定位到源代码错误的位置
inline-source-map //内联的source-map文件 不会生成单独的文件 只生成一个内联 功能:这个可以把错误定位到源代码准确位置
[可以隐藏原始代码]
```

```
总结 内联的构建比外联的更快
开发环境:速度快 调试更友好
速度(eval > inline > cheap) 所以 eval-cheap-source-map > eval-source-map
调试友好 source-map > cheap-module-source-map > cheap-map

总结 eval-source-map(vue默认) / eval-cheap-module-source-map

生产环境:源代码有必要隐藏？调试要不要更友好？ 
内联一般会让代码的体积非常大，一般排除
nosources-source-map 全部隐藏
hidden-source-map 隐藏源代码，会提示构建后代码的错误信息

总结 source-map
```



## 5.2 生产环境优化

* 优化打包构建速度
* 优化代码运行的性能