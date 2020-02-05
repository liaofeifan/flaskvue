# flaskvue
flask与vue前后端分离开发实验代码
# flask-vue前后端分离开发经验总结
## 1. 参考以下网址：
### (1)前端构建
https://www.jianshu.com/p/244113f59dce
### （2）前后端
https://codeburst.io/full-stack-single-page-application-with-vue-js-and-flask-b1e036315532
### （3）注意事项
- 用vue create project创建前端项目时，注意选择vue-router组件
- 前端项目中增加一个vue配置文件，主要解决flask中模板和静态文件生成目录问题，具体代码如下：

```
'use strict'

const IS_PROD = ["production", "prod"].includes(process.env.NODE_ENV);

const path = require('path');

module.exports = {
  // baseUrl从 Vue CLI 3.3 起已弃用，请使用publicPath
  // 默认情况下，Vue CLI 会假设你的应用是被部署在一个域名的根路径上，例如 https://www.my-app.com/。
  // 如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。例如，如果你的应用被部署在 https://www.my-app.com/my-app/，则设置 publicPath 为 /my-app/。
  publicPath: IS_PROD ? process.env.VUE_APP_PUBLIC_PATH : "./", // 默认'/'，部署应用包时的基本 URL
  // 当运行 vue-cli-service build 时生成的生产环境构建文件的目录。
  // 注意目标目录在构建之前会被清除 (构建时传入 --no-clean 可关闭该行为)。
  // 默认值'dist'
  outputDir: "dist",
  // 放置生成的静态资源 (js、css、img、fonts) 的目录(相对于outputDir目录)。
  // 默认值''
  assetsDir:"static",
  // assetsDir: "",
  // 指定生成的 index.html 的输出路径 (相对于 outputDir)。也可以是一个绝对路径。
  // 默认值'index.html'
  indexPath: "index.html",
  // indexPath: "index.html",
  // // 默认情况下，生成的静态资源在它们的文件名中包含了 hash 以便更好的控制缓存。
  filenameHashing: false,
  // 是否在开发环境下通过 eslint-loader 在每次保存时 lint 代码。这个值会在 @vue/cli-plugin-eslint 被安装之后生效。
  lintOnSave: process.env.NODE_ENV !== "production",

  //是否使用包含运行时编译器的 Vue 构建版本。设置为 true 后你就可以在 Vue 组件中使用 template 选项了，但是这会让你的应用额外增加 10kb 左右。
  runtimeCompiler: false,

  // 如果你不需要生产环境的 source map，可以将其设置为 false 以加速生产环境构建。
  productionSourceMap: false,

  // 所有 webpack-dev-server 的选项都支持。
  devServer: {
    host: "localhost",
    port: 8080, // 端口号
    https: false,
    open: true, //配置自动启动浏览器

    // 配置多个代理
    proxy: {
      "/api": {
        target: "http://localhost:3000", // 本地模拟数据服务器
        changeOrigin: true,
        pathRewrite: {
          "^/api": "" // 去掉接口地址中的api字符串
        }
      },
      "/foo": {
        target: "http://localhost:8080", // 本地模拟数据服务器
        changeOrigin: true,
        pathRewrite: {
          "^/foo": "" // 去掉接口地址中的foo字符串
        }
      }
    }
  }
};
```
- 后端项目中注意安装flask_cors，解决跨域访问问题
- flask后端项目中设置好文件夹路径，代码如下：
- 
```
# 此处静态文件文件夹和模板文件文件夹路径要与前端发布目录一致
app = Flask(__name__,
            static_folder="../fronted/dist/static",
            template_folder="../fronted/dist")
```

## 2.注意事项
### （1）vue版本采用的是最新版
### （2）vue-cli 默认生成的前端项目没有vue.config.js文件，需要手工在项目目录下创建一个


