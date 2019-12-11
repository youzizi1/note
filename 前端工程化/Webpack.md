## 打包工具

常见的打包工具有

* webpack
* rollup
* parcel

## Webpack

Webpack是一个模块打包器（bundler），它会将前端所有的资源文件，包括JavaScript，JSON，CSS和图片等作为模块，并根据模块之间的依赖关系进行分析，最后编译打包成对应的静态资源。

### 安装

```bash
npm i webpack webpack-cli
```

### 命令

* `webpack --mode`：指定打包模式
* `webpack --config`：指定配置文件
* `webpack --watch`：监听打包

### 配置文件

Webpack的所有操作都是基于默认的`webpack.config.js`配置文件。

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/index.js"
  },
  output: {
    filename: "dist.js",						// [name].js，即main.js
    path: path.resolve(__dirname, "dist")		// 必须是绝对路径
  }
};
```

然后，你可以直接通过`npx webpack`进行打包。

> 如果你没有编写该配置文件，webpack 4会默认将根目录下的`src/index.js`作为入口文件，构建出的文件为`dist/main.js`。

### 监听打包

你可以通过`webpack --watch`来进行监听打包，当然还可以通过配置文件来详细配置。

```js
module.exports = {
    watchOptions: {
        aggreateTimeout: 500, 			// 500ms内不打包
    	ignored: /node_modules/
    }
}
```

> 实际上，webpack自带的监听打包很少使用，一般使用devServer的监听打包。两者的区别是，前者会生成真实的打包文件，而后者不会生成真实的打包文件，而是在内存中。

### 打包模式

通过`webpack --mode`或者`mode`选项来配置打包模式。其中生产模式下，代码会进行压缩混淆，并且HRM和devServer会失效。

一般来说，我们会生成三个配置文件，分别为`webpack.base.js`，`webpack.dev.js`以及`webpack.prod.js`。其中，`webpack.base.js`是开发和生产环境下的公共配置，然后在开发和生产环境下的配置文件中通过`webpack-merge`模块来合并这个公共配置。

最后，你还需要配置`npm scripts`：

```json
{
    "scripts": {
        "dev": "webpack-dev-server --config webpack.dev.js",
    	"build": "webpack --config webpack.prod.js"
    }
}
```

### devServer

首先安装`npm i webpack-dev-server -D`，它可以实现`--watch`那样监听打包，还可以在实现自动打开浏览器以及文件变化后自动刷新浏览器等功能。

```js
module.exports = {
    contentBase: './dist',				// 设置静态文件目录
    open: true,
    progress: true						// 开启打包进度
    compress: true,						// 开启GZIP压缩，一般不配置
    proxy: {
        '/api': 'http://localhost:3000'	
    	// 访问/api/users将被代理到http://localhost:3000/api/users
    },
    port: 3000 							// 默认是8080
}
```

然后就可以通过`npx webpack-dev-server`来启动打包。

注意：通过`webpack-dev-server`打包生成的文件是处于内存中，一般配合`html-webpack-plugin`去自动引入打包后的文件。

> 打开浏览器是使用了`openBrowserPlugin`插件。

### Loader

Webpack默认只能处理JavaScript和JSON模块，其它模块要使用相应的loader来处理。

> Loader默认是从右向左执行，你可以配置`resloveLoader`改变这一规则。

#### 处理CSS

* `css-loader`：处理CSS文件
* `style-loader`：将处理好的CSS动态生成style标签插入到页面
* `sass-loader`：用来处理Sass和Scss，注意：需要先安装`node-sass`
* `less-loader`：用来处理Less

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/index.js"
  },
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      },
      {
        test: /\.s(css|ass)$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"]
      }
    ]
  }
};
```

默认情况下，`style-loader`生成的style标签会插入到head标签最底部，如果你希望覆盖模板中head标签已经定义的样式，可以通过`style-loader`的配置参数来实现。

```js
{
    test: /\.css$/,
    use: [
        {
            loader: 'style-loader',
            options: {
                insert: 'top'				// 插入head标签最顶部
            }
        }, 
        "css-loader"
    ]
}
```

另外，我们一般还需要对CSS属性自动添加前缀，此时就需要使用`postcs-loader`以及对应的插件`autoprefixer`。

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/index.js"
  },
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
            "style-loader", 		// 通过MiniCssExtractPlugin替换，抽离出单独的文件
            "css-loader",
            "postcss-loader"		// 一定要css-loader之前处理
        ]
      },
      {
        test: /\.s(css|ass)$/,
        use: [
            "style-loader", 
            "css-loader", 
            "postcss-loader",
            "sass-loader"
        ]
      },
      {
        test: /\.less$/,
        use: [
            "style-loader", 
            "css-loader",
            "postcss-loader",
            "less-loader"
        ]
      }
    ]
  }
};
```

当然你还要配置postcss。

```js
// postcss.config.js
const autoprefixer = require('autoprefixer')

module.exports = {
    plugins: [
        autoprefixer
    ]
}
```

#### 处理字体

通过`file-loader`来处理字体文件。

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/index.js"
  },
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
        {
        	test: /\.(eot|ttf|svg)/,
        	use: ['file-loader']
		}
    ]
  }
};
```

#### 处理图片

通过`url-loader`来处理图片文件。

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/index.js"
  },
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)/,
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 8192,						// 图片小于8kb，则使用Base64引入
              outputPath: "/images/"			// 输出文件夹
              // name: '[name]_[hash].[ext]'	// 自定义打包后的名称
            }
          }
        ]
      }
    ]
  }
};
```

注意：在JavaScript中使用图片需要通过`import`或`require`引入。

```js
import demo from "./demo.jpg"''

const img = new Image();
img.src = demo;
document.body.appendChild(img);
```

> file-loader也可以处理图片模块，但是更多的是使用url-loader。

对于CSS中引入的图片，`css-loader`会自动处理。

如果是在模板文件`index.html`中使用img标签的形式引入图片，你需要使用`html-withimg-loader`。

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        tets: /\.html$/,
        use: 'html-withimg-loader'
      },
      {
        test: /\.(png|jpg|gif)/,
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 8192,						// 图片小于8kb，则使用Base64引入
              outputPath: "images/"				// 输出文件夹
              // name: '[name]_[hash].[ext]'	// 自定义打包后的名称
            }
          }
        ]
      }
    ]
  }
};
```

#### 处理ESNext

安装`babel-loader`，`@babel/core`和`@babel/preset-env`。

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude： /node_modules/,
                include: path.resolve(__dirname, './src'),
            	use: {
              		loader: "babel-loader",
              		options: {
                		presets: ["@babel/preset-env"]
              		}
            	}
          	}
        ]
    }
}
```

其中，`babel-loader`是`webpack`和`babel`的通信桥梁，但是不会执行`ES6`编译工作，这部分由`@babel/preset-env`来实现。

但是上面配置并不会转义一些实例上的方法以及全局变量，例如`'aa'.includes('a')`以及`Promise`全局对象，此时你就需要使用`@babel/polyfill`。

我们可以在入口文件中直接引入`@babel/polyfill`，但是很多情况下，我们只想将用到的语法编译，而不是所有的语法，因此需要如下配置：

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude： /node_modules/,
                include: path.resolve(__dirname, './src'),
            	use: {
              		loader: "babel-loader",
              		options: {
                		presets: [
            				[
            					"@babel/preset-env",
            					{
            						useBuiltIns: 'usage'		// 只编译用到的语法
            					}
        					]
        				]
              		}
            	}
          	}
        ]
    }
}
```

这样你就不需要在入口文件中引入`@babel/polyfill`了。

当然你也可以编译到指定平台。

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude： /node_modules/,
                include: path.resolve(__dirname, './src'),
            	use: {
              		loader: "babel-loader",
              		options: {
                		presets: [
            				[
            					"@babel/preset-env",
            					{
            						useBuiltIns: 'usage'		// 只编译用到的语法
            						targets: {
            							chrome: '67',
            							safari: '11.1'
            							// ...
            						}
            					}
        					]
        				]
              		}
            	}
          	}
        ]
    }
}
```

上面的配置基本上可以直接应用到业务开了，但是如果开发第三方类库时，就会有一些问题。因为会将polyfill代码直接注入到全局，也就是说类似`window.Promise`（当然这个Promise是模拟的），这会污染全局。因此，在开发类库时，更希望将代码注入到局部。

因此，你需要使用`@babel/plugin-transform-runtime`和`@babel/runtime`。

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude： /node_modules/,
                include: path.resolve(__dirname, './src'),
            	use: {
              		loader: "babel-loader",
              		options: {
                		plugins: [
            				[
            					"@babel/plugin-transform-runtime",
            					{
            						'corejs': 2,		
                					'helpers': true,
            						'regenerator': true,
            						'useESModules': false
        						}
    						]
    					]
              		}
            	}
          	}
        ]
    }
}
```

当然，你也可以将babel配置写成单独的`.babelrc`文件。

```js
{
	presets: [
        [
            '@babel/preset-env',
            {
                targets: {
                    chrome: '67',
                    safari: '11.1'
                    //...
                },
            	useBuiltIns: 'usage'			
        	}
        ]
    ]		
}
```

> babel7.4已经废弃`@babel/polyfill`，最新文档看官网。

注意：最新的@babel/polyfill已经被废弃了，建议看官网使用最新的。

#### 处理vue文件

通过`vue-loader`来处理`.vue`文件。

```js
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  // ...
  module: {
    rules: [
       // ...
      {
        test: /\.vue$/,
        use: "vue-loader"
      }
    ]
  },
  plugins: [
    new VueLoaderPlugin()
  ]
};

```

这样你就可以直接在入口文件中写vue代码了。

```js
import Vue from 'vue'				// vue的package.json中main字段是vue.runtime.common.js

import App from './App.vue'

new Vue({
  render: h => h(App),
}).$mount('#app')
```

注意：你引入的vue并不包含compiler，只有runtime，所以你只能通过render来书写模板。如果你想要使用`template`去书写模板，你需要再安装`vue-template-compiler`来将template编译成render函数。`vue-loader`会自动调用这个插件，你并不需要额外的配置。

#### 处理JSX文件

```js
module.exports = {
	// ...
	module:{
		rules:[
			// ...
          	{
                test: /\.jsx$/,
                use: "babel-loader",
                exclude: /node_modules/
            }
		]
	}
}
```

另外，你还要配置相应的`babel`配置，具体看官网。

这样你就可以在入口文件中使用JSX语法了。

```jsx
import React, { Component } from 'react'
import ReactDom from 'react-dom'

class App extends Component {
  render() {
    return <div>hello world</div>
  }
}

ReactDom.render(<App />, document.getElementById('app'))
```

#### 处理TypeScript文件

可以通过ts-loader来打包TypeScript。注意：打包之前一定要新建tsconfig.json来配置ts，否则ts-loader会打包失败。

```js
//tsconfig.json
{
  "compilerOptions": {
    "sourceMap": true,
    "removeComments": true,
    "allowJs": true,
    "alwaysStrict": true,
    "checkJs": true,
    "target": "es5",
    "module": "es6"
  },
  "include": [
    "src/**/*"
  ]
  /*
  "exclude": [
  	"node_modules/"		// 不用配置，webpack已经配置
  ]
  */
}

// webpack.config.js
module.exports = {
    module: {
    	rules: [
      		{
        		test: /\.tsx?$/,
        		exclude: /node_modules/,
        		use: 'ts-loader'
      		}
    	]
  	},
 	resolve: {
    	extensions: [".ts", ".tsx", ".js"]
  	}
}
```

然后就能打包成功了。注意：使用了ts之后，如果需要引入第三方库，那么只需要引入它对应的类型文件即可。

```tsx
import * as _ from 'lodash'
```

如何知道哪些第三方库有types支持，可以到`https://microsoft.github.io/TypeSearch/`去搜索查看。

### Plugin

#### HtmlWebpackPlugin

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
    // ...
    plugins: [
        new HtmlWebpackPlugin({
          template: "./index.html",			// 设置模板
          filename: "index.html",			// 设置文件名
          minify: {
            removeComments: true,			// 去除注释
            removeAttributeQuotes: true,	// 去除属性引号
            collapseWhitespace: true		// 折叠空白符，即一行显示
          }
          // hash: true						// 更多的是文件名使用[name].[hash:8].js 
        })
  	]
}
```

#### CleanWebpackPlugin

```js
const CleanWebpackPlugin = require("clean-webpack-plugin");

module.exports = {
    // ...
    plugins: [
        new CleanWebpackPlugin()			// 自动清除dist文件夹
  	]
}
```

#### MiniCssExtractPlugin

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.css/,
                use: [
                    // 抽离出CSS文件通过link标签引入，而不是通过style-loader的style标签
                    MiniCssExtractPlugin.loader,	
                    'css-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.scss$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'sass-loader'
                ]
            }
        ]
    }，
    plugins: [
    	new MiniCssExtractPlugin({
    		filename: 'main.css'
    		// filename: 'css/main.css'		// 输出到指定文件夹
		})
    ]
}
```

#### 压缩静态文件

* `optimize-css-assets-webpack-plugin`：压缩CSS
* ` uglifyjs-webpack-plugin `：压缩JavaScript

这两个插件使用具体看文档。

#### BannerPlugin

```js
const webpack = require('webpack')

module.exports = {
    // ...
    plugins: [
    	new webpack.BannerPlugin('created at 2019.09.09 by ugu')
    ]
}
```

### CDN

很多情况下，我们希望打包的静态资源能够加上CDN路径，你只需要配置publicPath即可。

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/index.js"
  },
  output: {
    filename: "dist.js",						
    path: path.resolve(__dirname, "dist"),
    publicPath: 'http://www.mycdn.com'
  }
};
```

如果你只是想对图片资源配置CDN，你只需要配置图片Loader的publicPath即可。

```js
module.exports = {
    module: {
        rules: [
          {
            test: /\.(png|jpg|gif)/,
            use: [
              {
                loader: "url-loader",
                options: {
                  limit: 8192,						
                  outputPath: "/images/",			
                  publicPath: 'http://www.cdn.com'
                }
              }
            ]
          }
        ]
  }
}
```

### HMR

HMR，又称热模块更新。该功能一般是在开发是开启，需要结合devServer使用，具体配置如下：

```js
const webpack = require('webpack')

module.exports = {
    // ...
    devServer: {
        contentBase: './dist',
        open: true,
        hot: true,  			// 开启HMR
        hotOnly: true 			// 即便HMR没生效，也不让浏览器自动刷新，需要手动刷新
    },
    plugins: [
		new webpack.HotModuleReplacementPlugin()
	]
}
```

webpack配置好之后，还需要在模块中配置：

```js
import number from './number'
import count from './count'

number()
count()

if(module.hot) {
    module.hot.accept('./number', ()=>{
        number()			// 如果开启了HRM，那么文件变化时，只执行number，而不是刷新整个页面
    })
}
```

模块中的这些配置，vue和react的脚手架都自动配置好了，你只需要直接使用就行，不需要自己手写。

> * 对于CSS文件，如果开启了HMR，`css-loader`会自动帮你实现
> * 通过`webpack.NamedModulesPlugin`可以打印出HRM的模块

### sourceMap

source map是一种源代码和生成文件之间的映射关系文件。

```js
module.exports = {
    // ...
    devtool: 'source-map'
}
```

`devtool`选项还有如下值：

* `source-map`：单独生成一个文件，大而全
* `eval-source-map`：不会产生单独的文件，集成在打包后的文件，可以显示行和列
* `cheap-module-source-map`：不会产生列，但是是一个单独的文件
* `cheap-module-eval-source-map`：不会产生文件，集成打包后的文件，不会产生列

最佳实践：开发模式，建议使用`cheap-module-eval-source-map`；生产模式，建议使用`cheap-module-source-map`。

### ESLint

首先安装ESLint。

```bash
npm i eslint -D
```

然后通过`npx eslint --init`初始化ESLint配置，接着你就可以通过`npx eslint src/`来检测你的代码了。

当然使用webpack的时候，我们希望打包的时候自动进行代码检测。这时你就需要使用`eslint-loader`。

```js
module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: [
                    'babel-loader',
                    {
                        loader: 'eslint-loader',
                        options: {
                            fix: true,		// 简单的错误修复
                            enforce: 'pre'	// 始终先进行eslint检查
                        }
                    }
                ]
            }
        ]
    }
}
```

此时你执行`npx webpack`，如果打包出现代码风格错误，就会在命令行中显示。但是，很多情况下，我们更希望在错误在页面上显示，你需要进行如下配置：

```js
module.exports = {
    // ...
    devServer: {
        // ...
        overlay: true
    }
}
```

注意：像支持ESLint的编辑器，如果你安装了ESLint插件，它会自动读取项目中ESLint配置进行代码校验，这样也可以实现自动代码校验。另外，我们还可以通过Git Hook来实现提交代码前的代码校验。

### 第三方包

实际开发中，一般我们会引用第三方模块来协助开发，例如jQuery。

对于一些高频使用的模块，我们希望在每个模块中自动注入，而不是每次都手动引入，此时你需要进行如下配置。

```js
// webpack.config.js
import webpack from 'webpack'

module.exports = {
    plugins: [
        new webpack.ProvidePlugin({
            $: 'jquery'				// 自动注入$，无法通过window.$获取
        })
    ]
}
```

配置好之后，为了防止你在业务中又再次引入jQuery导致打包两次，所以还要配置如下：

```js
module.exports = {
    external: {
        jquery: '$'			// 忽略jQuery打包
    }
}
```

当然，一些第三方模块，你希望通过CDN导入，此时你可以直接在`index.html`模板引入jQuery路径，然后你就可以在模块中直接使用`$`或`window.$`。注意：最好还是配置一下`external`，防止你在模块中二次导入。

> 还有通过`expose-loader`将某个第三方包暴露到全局，但是不建议使用。

### 多页面打包

```js
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  mode: "development",
  // 配置多入口
  entry: {
    home: "./src/home.js",
    list: "./src/list.js"
  },
  output: {
    filename: "[name].js",						
    path: path.resolve(__dirname, "dist")		
  },
   plugins: [
     // 建议通过JavaScript重写
     new HtmlWebpackPlugin({
         template: './index.html',
         filename: 'home.html',
         chunks: ['home']
     }),
     new HtmlWebpackPlugin({
         template: './index.html',
         filename: 'list.html',
         chunks: ['list']
     })
   ]
};
```

### 配置别名

```js
module.exports = {
    resolve: {
        alias: {
        	@: 'src/'
    	},
        extensions: ['.js', '.json', '.vue'],	// 配置查找规则
    	mainFiles: ['index', 'child'],			// 默认寻找目录下的index，其次是child
        mainFields: ['main', 'style']			// 默认寻找package.json中main字段，其次style
    }
}
```

### 环境变量

通过`DefinePlugin`插件来注入全局变量。

```js
module.exports = {
    // ...
    new webpack.DefinePlugin({
    	ENV: JSON.stringify('development')		// 或者写成"'development'"
	})
}
```

这样你就可以在业务模块中使用这个全局变量了。

```js
let url = ''

if(ENV === 'development') {
    url = 'http://dev.com'
}else {
    url = 'http://prod.com'
}
```

### Tree Shaking

webpack在生产环境下会自动进行Tree Shaking优化。所谓Tree Shaking优化，就是会自动删除一些冗余代码。例如：A模块中有两个函数，但是你只用到了其中一个，那么在打包的时候，Webpack会自动删除未使用的代码。

当然，不是所有的模块都能Tree Shaking的，需要满足四个条件：

* 使用`ES6`模块导入，因为`import`属于静态导入，`require`属于动态导入
* 确保`ES6`模块没有被 babel 等编译器转换成`CommonJS`的形式
* `package.json`要有 `"sideEffects"` 属性的定义
* `mode`开启为`production`

另外，webpack还会做一些简单的小优化，例如：

```js
console.log(1 + 1);

// 打包后

console.log(3)
```

### 代码分割

通过`SplitChunksPlugin`来实现JavaScript代码分割。

```js
module.exports = {
	// ...
    optimization: {
        // 配置项很多，具体看官网
        splitChunks: {
            // 模块超过30kb才会被打包，会被cacheGroups中的属性继承
            minSize: 30000
            cacheGroups: {
                // 抽离多次引入的业务模块
                default: {						// 定义分割规则，可以不叫default
                    name: 'common',				// 抽离出来的模块名，若不配置webpack会自动生成
                    chunks: 'initial',			// 只支持同步模块抽离，异步模块需要使用async
                    minChunks: 2,
                    priority: -20
            		// 可以配置minSize进行覆盖
                },
                // 抽离多次引入的第三方模块 
                vendors: {
                    test: /node_modules/,
                    name: 'vendor',
                    chunks: 'intial',			// all代表同时支持同步和异步模块抽离。
                    priority: -10
                }
            }
        }
    },
    output: {
        filename: 'dist.js',
        chunkFilename: '[name].js',		
        path: path.resolve(__dirname, './dist')
    }
}
```

注意：异步模块导入需要通过Babel的`@babel/plugin-synatax-dynamic-import`插件来转义。下面代码是模块中使用异步导入模块：

```js
function getComponent() {		
    return import(/* webpackChunkName:"lodash" */ 'lodash').then(({default: _}) => {
        const ele = document.createElement('div')
        ele.innerHTML = _.join(['hello', 'world'], '-')
        return ele
    })
}

getComponent().then(ele => {
    document.body.appendChild(ele)
})
```

然后你需要进行配置`chunks`为`async`或`all`即可。

实际开发中，往往是需要抽离公共代码的，尤其是多页面打包时，具体优点如下：

* 公共部分便于缓存
* 提交页面加载速度，一般来说加载两个小chunk比一个大chunk时间要小

> CSS代码一般也要做代码分割，一般是`splitChunks`配合`MiniCssExtractPlugin`来实现，具体看文档。

### 打包分析

可以通过`npx webpack --profile --json > stats.json`生成打包的JSON文件，然后在官方提供的`http://webpack.github.io/analyse/`中导入这个文件进行分析。

当然，你也可以使用第三方插件`webpack-bundle-analyzer`来分析打包。

当然，我们更推荐使用webpack-bundle-analyzer来分析。

> 可以在`https://webpack.js.org/guides/code-splitting/#bundle-analysis`这里查看更多的分析过程。

### 打包优化

#### noParse

如果你引入的第三方模块没有依赖其它模块，可以配置noParse，来让webpack不去分析这个模块的依赖关系。

```js
module.exports = {
    // ...
    module: {
        noParse: /jquery/,
        rules: [
            // ...
        ]
    }
}
```

#### ignorePlugin

如果你使用moment之类的库，可能会全局引入所有地区的语言包。而实际上，我们只需要自己手动引入本地区的语言包即可，此时你就需要使用`ignorePlugin`。

```js
const webpack = require('webpack')

module.exports = {
    // ...
    plugins: [
        new webpack.IgnorePlugin(/\.\/locale/, /moment/)	// 忽略moment引入的locale
    ]
}
```

配置好之后，你需要在模块中手动引入所需的语言包。

#### happypack

```js
const happypack = require('happypack')

module.exports = {
    // ...
    module: {
        rules: [
            // ...
            {
                test: /\.cs$/,
                use: 'happypack/loader?id=css'		// 对css进行多线程打包
            }
        ]
    },
    plugins: [
        new happypack({
            id: 'css',
            use: ['style-loader', 'css-loader']
        })
    ]
}
```

#### 总结

当然还有一些其它优化点：

* 提高Node和Webpack的版本
* 尽量减少模块应用Loader。例如：打包JavaScript模块时，要配置exclude
* 合理使用sourceMap
* 控制包大小。例如：使用tree-shaking去掉冗余代码以及使用split chunk进行代码分割
* 拆分开发环境以及生产环境配置
* 结合stats分析打包结果
* 通过dllPlugin进行打包优化
* 开启Tree Shaking

### 模块懒加载

```js
// a.js
export default = 'hello world'

// b.js
const button = document.createElement('button')

button.addEventListener('click', () => {
    // 动态导入需要通过Babel转义
    import('./a.js').then(data => {		// 可以通过魔法注释来定义chunkName
        console.log(data.default)
    })
})

document.body.appendChild(button)
```

上面代码只有在点击时才会加载`a.js`模块，这就是我们所说的懒加载，也是React和Vue中组件懒加载的原理。

注意：与webpack无关，对于图片也有懒加载以及预加载。

**图片懒加载**

针对图片定义，当进入可视区域之后再请求图片资源。

优点：

* 减少无效资源的加载，甚至不加载，减少服务器压力
* 并发加载的资源过多会阻塞JavaScript脚本的加载，虽然JavaScript脚本加载优先级高于图片

实现原理：将真实图片URL放入img标签的data-src属性中，然后检测scroll和resize事件，判断图片进入可视范围，将data-src存储的真实路径赋值给src属性。

> Vue中可以使用`vue-lazyload`模块来实现。

**图片预加载**

提前加载图片，避免一些页面因为图片缺失而造成的体验优化。

实现原理：

* 通过CSS的`background`属性

  ```css
  #preload-01 { 
      background: url(http://domain.tld/image-01.png) no-repeat -9999px -9999px; 
  } 
  ```

* 通过JavaScript的`new image()`的`onload`属性

**区别**

* 懒加载可以减少服务器压力
* 预加载会增加服务器压力

### 提前加载和预判加载

webpack认为异步模块能提高网页加载速度，所以`chunks`的默认配置是`async`。

但是异步模块有一个问题，就是只有在交互的时候才会加载该模块，影响用户体验。因此提前加载`preload`和预判加载`prefetch`加载方式出现了。前者表示会随着主要业务资源一起加载，后者会等到主要资源加载后再加载。

```js
const button = document.createElement('button')

button.addEventListener('click', () => {
    // 开启prefetch，建议使用prefetch		
    import(/*webpackPrefetch: true*/'./a.js').then(data => {
        console.log(data.default)
    })
})

document.body.appendChild(button)
```

注意：`prefetch`太多会导致浏览器崩溃。

> 打开Chrome调试工具，按下`Ctrl+Shift+P`，搜索`coverage`，可以查看当前脚本的利用率。那些未被利用的红色代码，一般来说都可以通过异步来改写。

### Library

将自己的代码打包成一个第三方模块，供别人使用。

```js
module.exports = {
    // ...
    library: 'lib',		// 全局暴露lib变量
    library: 'umd'		//  打包成umd模块
}
```

### PWA打包

通过`workbox-webpack-plugin`插件来将你的应用添加PWA特性。

```js
module.exports = {
    // ...
    plugins: [
        new WorkboxPlugin.GenerateSW({
            clientsClaim: true,
            skipWaiting: true
        })
    ]
}
```

### 自动部署

通过`uploadPlugin`把打包后的代码自动上传的服务器。

### 自定义Loader

Loader其实就是一个函数，接收一个source参数，表示要处理模块的源码。

```js
module.exports = function(source) {
    return source.replace('hello', 'hi')	// 将源码中的hello转换为hi
}
```

然后你就可以配置使用了。

```js
const path = require('path')

module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.js$/,
                use:[
                    path.resolve(__dirname, './loader/hanle-index-loader.js')
                ]
            }
        ]
    }
}
```

当然，你还可以再Loader中配置参数：

```js
const path = require('path')

module.exports = {
    // ...
    module: {
        rules: [
            {
                test: /\.js$/,
                use: [
                    {
                       loader: path.resolve(__dirname, './loader/hanle-index-loader.js'),
                       options: {
                           name: 'ugu'
                       }
                    }
                ]
            }
        ]
    }
}
```

这样在Loader中就需要使用`this.query.name`来获取参数值。当然，我们还是更建议使用`loader-utils`模块来获取参数。

```js
const loaderUtils = require('loader-utils')

module.exports = function(source) {			
    const options = loaderUtils.getOptions(this)
    return source.replace('hello', options.name)
}
```

此时如果sourceMap分析，你希望同样返回去，而不是仅返回源码。此时，你就需要使用`this.callback`，该函数接收四个参数，分别是`err`，`content`，`sourceMap`和`meta`。

```js
const loaderUtils = require('loader-utils')

module.exports = function(source) {			
    const options = loaderUtils.getOptions(this)
    return source.replace('hello', options.name)
    this.callback(null, result)		// 没有错误为null，content和sourceMap没有就不传递
}
```

如果Loader中有异步操作，你需要使用`this.async`来处理。具体使用看官网。

### 自定义Plugin

和Loader不同，Plugin是一个类。

```js
class MyPlugin {
    // 插件的参数
    constructor(options) {
        console.log(options)
    }
    
    // 必须有一个apply方法，参数表示webpack实例。这个实例有一些钩子，例如打包开始，打包结束等
    apply(compiler) {
        compiler.hooks.emit.tapAsync('MyPlugin', (compilation, cb) => {
            // 打包时新增demo.txt文件，内容为hello world，大小为11
            compilation.assets['demo.txt'] = {
                source: function() {
                    return 'hello world'
                },
                size: function() {
                    return 11
                }
            }
            cb()
        })  
    }
}

module.exports = MyPlugin
```

这样就可以在webpack配置写好的自定义插件了。

> Plugin的实现主要依赖`taptable`库，具体实现看官网。

### 内部原理

#### 打包原理

```js
(function(modules) {
  function require(moduleId) {
    modules[moduleId]();
  }
  require("./index.js");
})({
  "./index.js": function(module, exports) {
    eval("console.log('hello world')\n\n//# sourceURL=webpack:///./index.js?");
  }
});
```

当然里面还有缓存，模块标识`id`等等。

#### 实现bundle

```js
const fs = require("fs");
const path = require("path");
const babylon = require("@babel/parser");
const traverse = require("@babel/traverse").default;
const babel = require("@babel/core");

// 标记模块的ID
let ID = 0;

// 分析依赖关系
function createAsset(filename) {
    // 读取文件内容
    const content = fs.readFileSync(filename, "utf-8");
	// 生成AST
  	const ast = babylon.parse(content, {
    	sourceType: "module"
  	});
    
    // 存储依赖关系，最终是['./a.js', './b.js']
    const dependencies = []
    
    // 遍历AST，将依赖放到dependencies中
    traverse(ast, {
    	// 获取import对应的节点
    	ImportDeclaration: ({ node }) => {
            dependencies.push(node.source.value);
    	}
  	});
    
    const id = ID++;

  	// ES6代码转换ES5
  	const { code } = babel.transformFromAstSync(ast, null, {
    	presets: ["@babel/preset-env"]
  	});
    
    return {
        id,
        filename,
        dependencies,
        code
    };
}

// 从入口开始分析模块依赖项，形成依赖图，采用广度遍历
function createGraph(entry) {
    // 获取入口文件的信息，包括id, filename, dependencies以及code
    const mainAsset = createAsset(entry);
   
  	const queue = [mainAsset];
    
    for (const asset of queue) {
    	const dirname = path.dirname(asset.filename);

    //新增一个属性来保存子依赖项的数据
    //保存类似 这样的数据结构 --->  {"./message.js" : 1}
    asset.mapping = {};

    asset.dependencies.forEach(relativePath => {
      const absolutePath = path.join(dirname, relativePath);
        
      //获得子依赖的信息，包括id, filename, dependencies以及code
      const child = createAsset(absolutePath);

      // 给子依赖项赋值，
      asset.mapping[relativePath] = child.id;

      //将子依赖也加入队列中，进行广度遍历
      queue.push(child);
    });
  }
  return queue;
}

// 根据生成的依赖关系图，生成对应环境能执行的代码，目前是生产浏览器可以执行的
function bundle(graph) {
  let modules = "";

  // 循环依赖关系，并把每个模块中的代码存在function作用域里
  graph.forEach(mod => {
    modules += `${mod.id}:[
      function (require, module, exports){
        ${mod.code}
      },
      ${JSON.stringify(mod.mapping)},
    ],`;
  });
   

  // 自定义require，以及module.exports
  const result = `
    (function(modules){
      function require(id){
        const [fn, mapping] = modules[id];
        function localRequire(relativePath){
          //根据模块的路径在mapping中找到对应的模块id
          return require(mapping[relativePath]);
        }
        const module = {exports:{}};
        //执行每个模块的代码。
        fn(localRequire,module,module.exports);
        return module.exports;
      }
      //执行入口文件，
      require(0);
    })({${modules}})
  `;
    /*
    	modules的值类似：
    	{
  			0: [
                function(require, module, exports) {
                    "use strict";
                    console.log("hello world");
                },
    			{}
  			]
		}
    */
    
    return result;
}

const graph = createGraph("./example/entry.js");
const ret = bundle(graph);

// 打包生成文件
fs.writeFileSync("./bundle.js", ret);
```

## Babel

* @Babel/node
* @Babel/core
* @Babel/cli
* @Babel/polyfill
* @Babel/runtime
* @babel/plugin-transform-runtime 

## 脚手架

* commander：命令行工具
* ora：实现命令行loading效果
* inquirer：实现用户与命令行交互的模块
* download-git-repo：实现从github或gitlab上下载代码
* Oclif：基于Node.js开发CLI工具框架
* yargs：处理命令行参数
* 

## 抽象语法树

`AST`（`Abstract Syntax tree`）常用来实现一些编译工具和lint工具，例如`babel`，`ESLint`等。

具体过程：

* 解析代码生成`AST`
* 遍历`AST`
* 修改`AST`
* 生成新的代码

其中，通过`esprima`模块可以实现对`JavaScript`代码解析成`AST`，通过`estraverse`模块实现对`AST`遍历并修改，通过`escodegen`模块将修改后`AST`生成对应的代码。

在`babel`中，你可以通过`@babel/core`模块的`transform`方法将代码转换为`AST`，通过`@babel/types`模块实现`AST`遍历以及生成新的代码。

下面是手动实现`ES6 Class`语法转换成`ES5 Function`的插件：

```js
const babel = require("@babel/core");
const types = require("@babel/types");

const code = `
  class Person {
    constructor(name,age) {
      this.name = name
      this.age = age
    }
    getName() {
      return this.name
    }
  }
`;

const ClassPlugin = {
  visitor: {
    ClassDeclaration(path) {
      const node = path.node;
      // 获取类名
      const className = node.id.name;

      // 获取类中的函数
      const funcList = node.body.body;

      // 声明一个空函数，函数名为获取到的类名
      const newFun = types.functionDeclaration(
        types.identifier(className),
        [],
        types.blockStatement([]),
        false,
        false
      );
      path.replaceWith(newFun);

      const newFunArr = [];

      funcList.forEach((item, index) => {
        // 获取每个函数的函数体
        const body = funcList[index].body;
        // 如果是构造函数，就要生成新的函数，否则就要放到原型上
        if (item.kind === "constructor") {
          const newFun = types.functionDeclaration(
            types.identifier(className),
            item.params,
            body,
            false,
            false
          );
          newFunArr.push(newFun);
        } else {
          const protoObj = types.memberExpression(
            types.identifier(className),
            types.identifier("prototype")
          );
          // Person.prototype.left = right
          const left = types.memberExpression(
            protoObj,
            types.identifier(item.key.name)
          );
          const right = types.functionExpression(
            null,
            item.params,
            body,
            false,
            false
          );

          const assign = types.assignmentExpression("=", left, right);
          newFunArr.push(assign);
        }
      });
      // 拼接构造函数和普通函数
      if (newFunArr.length) {
        path.replaceWithMultiple(newFunArr);
      }
    }
  }
};

const newCode = babel.transform(code, {
  plugins: [ClassPlugin]
});

console.log(newCode.code)
```

对于`babel-plugin-import`插件，实际上就是通过`AST`将`import {Button} from 'antd'`转换成`import Button from 'antd/lib/button' `语法，这样就实现了按需加载。