# vue-practice

```
$ cd vue-webpack
$ npm install

# develop
$ npm run dev

# production
$ npm run build
```

## [使用vue开发项目梳理及需要掌握的知识点](http://reahink.com/2017/09/%E4%BD%BF%E7%94%A8vue%E5%BC%80%E5%8F%91%E9%A1%B9%E7%9B%AE%E6%A2%B3%E7%90%86%E5%8F%8A%E9%9C%80%E8%A6%81%E6%8E%8C%E6%8F%A1%E7%9A%84%E7%9F%A5%E8%AF%86%E7%82%B9/)


## 过程

__vue项目目录结构__
```
vue-webpack				vue项目
    │
    ├──build				webpack配置文件夹
    │   │
    │   ├──build.js				 生产环境构建
    │   ├──check-versions.js		检查node、npm版本
    │   ├──dev-client.js			开发模式下用到的工具，如：热重载
    │   ├──dev-server.js			开发模式下，构建本地服务
    │   ├──utils.js				 构建工具相关，封装了webpack模块用到的方法
    │   ├──vue-loader.conf.js	   .vue单文件加载配置
    │   ├──webpack.base.conf.js	 webpack基础配置
    │   ├──webpack.dev.conf.js	  webpack开发环境配置
    │   ├──webpack.prod.conf.js	 webpack生产环境配置
    │
    ├──config				webpack配置所用变量
    │   ├──dev.env.js				开发环境
    │   ├──index.js					webpack配置文件
    │   ├──prod.env.js				生产环境
    │
    ├──src 					源文件夹
    ├──static 				静态文件夹
    ├──.babelrc				babel编译配置
    ├──.editorconfig		在IDE中提供代码一致性
    ├──.postcssrc.js		通过JS插件装换样式的工具
    ├──index.html			webpack插件HtmlWebpackPlugin生成html所用的模版
    ├──package.json			项目信息及依赖
```
默认情况下：

```
# 构建生产代码
$ npm run build
```
vue-webpack目录下会生成```dist```文件夹

### 3.更改生产代码输出目录
更改在```config```文件夹下的```index.js```

```
index: path.resolve(__dirname, '../../myapp/dist/index.html'),
assetsRoot: path.resolve(__dirname, '../../myapp/dist'),
```
最终代码输出目录为：

```
	 vue-practice
        │
        ├──vue-webpack			vue项目
        │   │
        │
        ├──myapp
        │   │
        │   ├──dist
        │   │   ├──index.html
        │   │   ├──static

```
这个时候：
```
$ cd myapp
$ npm install
# 启动node服务
$ node bin/www
```
在浏览器中访问：http://localhost:3000/

### 4.开发环境下与测试接口进行数据交互

在```vue-webpack```下安装以下模块

```
$ npm i cookie-parser -D
$ npm i body-parser -D
$ npm i request -D
```

在build文件夹下将安装的模块引入```dev-server.js```

```
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var request = require('request');

//找到下面的两个所在的位置
var app = express()
var compiler = webpack(webpackConfig)

//在上面的下面引入以下
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());

app.all('/路由地址/*',function (req, res, next) {
  //测试接口地址
  var path = 'http://xxx.xxxx.com'+req.path;
  let params =req.body;
  console.log(params);
  console.log(path);
  request({
    url: path,
    method: "POST",
    json: true,
    body: params
  }, function (error, response, body) {
    if (error) {
      console.log(error);
    } else {
      console.log(typeof body);
      res.send(body)
    }
  });

})
```

### 5.使用[axios](https://github.com/mzabriskie/axios)进行AJAX请求
安装

```
$ npm i axios -D
```
写法：

```
axios.post('接口地址', {键值对对象})
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```
