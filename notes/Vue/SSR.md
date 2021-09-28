### SSR

##### What

Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。

服务器渲染的 Vue.js 应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在**服务器**和**客户端**上运行。

##### CSR VS SSR

传统的web开发，客户端向服务端发送请求，服务端查询数据库，拼接HMTL字符串（模板），通过一系列的数据处理之后，把整理好的HMTL返回给客户端，浏览器相当于打开一个页面。如JSP等。

SPA应用，页面整体是JS渲染出来的，称之为客户端渲染CSR。SPA渲染过程中，由客户端访问URL发送请求到服务端，返回HTML结构（很小，只有一个基本结构）。客户端接收到返回结果之后，在客户端开始渲染HTML，渲染时执行对应JS，最后渲染template，渲染完之后，再次发送数据请求，客户端接收数据并完成渲染。

CSR减轻服务器压力，也是有缺点的：

1. 首屏渲染时间较长：必须等JS加载执行完毕

2. 不利于SEO：如Vue，只有div

为了解决以上两个问题，出现了SSR，后端渲染出首屏的DOM结构返回，前端拿到内容带上首屏，后续的页面操作，再用单页面路由和渲染。

SSR的渲染流程：客户端发送URL请求到服务端，服务端读取对应的URL的模板信息，在服务端做出HTML和数据的渲染，渲染完成之后返回HTML结构，客户端拿到首屏页面的HTML结构。所以用户在浏览首屏时速度会很快，因为客户端不需要再次发送Ajax请求。

SSR是处于CSR与SPA之间的一个这种方案，渲染**首屏**的时候在服务端做出渲染，其他还是在客户端渲染的。在服务端接收到请求之后并且渲染出首屏页面,会携带着剩余的路由信息预留给客户端去渲染其他路由的页面.

##### SSR　Advantages

1. 更好的SEO
2. 更快的内容到达时间，特别是对于缓慢的网络情况或运行缓慢的设备

##### SSR　Disadvantages

1. 开发条件受限：生命周期的钩子函数使用，外部扩展库可能需要特殊处理
2. 构建设置和部署的更多要求
3. 更多的服务器端负载

##### When

**内容到达时间的重要性！！！**

自我感觉：实用于需要靠搜索引擎带来流量的项目。

##### 服务端渲染 VS 预渲染

如果只是用来改善少数页面的SEO，**预渲染**可能是个不错的选择。在构建时简针对特定路由生成静态HTML。

优点：设置预渲染更加简单，并可以将你的前端页面作为一个完全静态的站点。

[prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin)

##### [vue-server-renderer](https://ssr.vuejs.org/zh/guide/#%E5%AE%89%E8%A3%85)

##### Nuxt特点

1. 基于Vue
2. 自动代码分层
3. 服务端渲染
4. 强大的路由功能,支持路由数据
5. 静态文件服务
6. ES6和ES7的语法支持
7. 打包和压缩JS和CS
8. HTML头部标签管理
9. 本地开发支持热加载
10. 集成ESLint
11. 支持各种样式预编译器Less,Sass等

##### 环境搭建

```
npm i create-nuxt-app -g
npx create-nuxt-app <project-name>
cd <project-name>
npm run dev
```

##### 渲染流程

![20190702173158457](https://user-images.githubusercontent.com/23520842/135073951-00f633e6-105a-4169-90ba-a685d2e0809c.png)



##### 目录结构

**建议:文件夹名字不要随意更改**

| -- .nuxt        // 自动生成,临时编译/build的文件

| -- assets      // 未编译的静态资源,如less,JS等

| -- components	// 通用组件,会自动注册

| -- layouts	// 布局组件

​	| -- default.vue

​	| -- error.vue

​	| -- XX.vue

| -- middlewares	// 中间件,路由加载之前加载

| -- pages	// 路由及视图,根据目录结构自动生成路由

​	| -- index.vue

​	| -- user

​		|-- index.vue

​		| -- one.vue

​		| -- _id.vue

​	| -- _two

​		| -- index.vue

| -- plugins	// Vue实例化之前需要应用的JS插件，使用时可区分环境使用

| -- static	// 静态资源,不需要webpack编译,服务器启动后自动映射到根目录下

| -- store	// Vuex状态管理

| -- .editorconfig	// 开发工具格式配置

| -- .eslintrc.js	// eslint配置

| -- .gitignore	// git忽略文件

| -- nuxt.config.js	// 个性化配置

| -- package-lock.json

| -- package.json

| -- README.md

```
routes: [
	{
		path: '/',
		name: 'index',
		component: 'pages/index.vue'
	},
	{
		path: '/user',
		name: 'user',
		component: 'pages/user/index.vue'
	},
	{
		path: '/user-one',
		name: '/user/one',
		component: 'pages/user/one.vue'
	},
	// 可选
	{
		path: '/user-id',
		name: '/user/:id?',
		component: 'pages/user/_id.vue'
	},
	// 必填
	{
		path: '/:two',
		name: 'two',
		component: 'pages/_two/index,vue'
	}
]
```

##### 部署

node环境

1. ```npm run build```,生成```.nuxt```文件夹

2. 将```.nuxt,  nuxt.config.js,  package.json,  package-lock.json,  static```放在服务器上,执行```npm install ,npm start```

3. 建议不要执行```npm install --production```

   
