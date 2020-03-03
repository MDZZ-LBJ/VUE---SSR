# VUE---SSR
### 什么是服务端渲染 ？
  ssr （ server side render )
  把vue的组件在服务端变成HTML字符串，将他们直接发送到浏览器，最后将这些静态标记
 ‘激活’变成可交互的应用程序 

### 为什么使用服务端渲染（SSR）？
	 
 1 更利于SEO优化，能够抓取到整个野蛮
	
 2 更快的内容达到时间（加快首屏渲染），不需要解析js


   

### 服务端渲染的权衡

 1 开发条件的限制，组件中某些生命周期钩子不能用，列如：created，mounted的等，即beforMounted之后都不行。
	
 2 开发环境的限制，大多依赖node环境
  
 3 会加大服务器负载，因为是在后端渲染页面 ，并且每次请求都会返回一个独立的vue实例，
 
### 这里基于 vue3.x 即 @vue/cli 

### 创建工程
  vue init webpack ssr

### 安装依赖
  cnpm install vue-server-renderer （渲染器）
  
  cnpm install express  （服务器，这里用node的后台）
  
  cnpm install cross-env （跨平台打包脚本） 
  
  cnpm install webpack-node-externals （webpack优化配置）
  
  cnpm install lodash.merge （合并bundle文件）

### 构建流程

 server bundle ：是用来处理前端请求对应的页面，将其编译成html。
 
 client bundle ：是用来打包一些和这个页面有关的代码，用于将这个静态的html页面激活
                      成一个可用的spa页面。 
  
 所以：将来webpack打包的时候要有两个入口文件，生成两个包 即 server bundle ，client bundle，
      最后返回给前端(浏览器)的是这两个包结合生成的html的字符串，然后我们再去激活

### 关键步骤

 1 修改路由 将导出路由实例改为导出创建路由实例函数

 2 新建app.js公共入口文件，导出创建App和路由实例的函数 弃用main.js入口文件 

 3 新建 entry-server.js 服务端入口文件，供node服务器调用，可以拿到服务端的路由
   push到路由实例中 ，首屏渲染就是这样实现的

 4 新建 entry-client.js 客户端入口文件，用于挂载激活

 5 配置webpack，分别生成服务端和客户端的文件，将服务端的bundle文件和客户端的清单合并
   插入到模板中，将路由权限交给vue-router
