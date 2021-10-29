Vue的路有懒加载在开发环境中热加载效率非常低，所以需要区分环境。

##### 环境：Vue-cli3.X, vue-loader 13.0.0+

##### 旧方案

1. 在`router`文件夹下创建`_import_development.js,_import_production.js`

   ```
   // _import_development.js
   module.exports = file => require(`@/views/${file}.vue`).default;
   // _import_production.js
   module.exports = file => () => import(`@/views/${file}.vue`);
   ```

2. `router.js`根据环境引入相应的JS文件

   ```
   // router.js
   const _import = require(`./_import_${process.env.NODE_ENV}.js`);
   const routes = [
   	{
   		path: '/home',
   		name: 'Home',
   		component: _import('Home/index')
   	}
   ];
   ```

   ##### 新方案

   使用`babel`的插件—`babel-plugin-dynamic-import-node`。它将所有的`import()`转换为`require()`，这样就可以用这个插件将所有异步组件都用同步的方式引入，结合`BABEL_ENV`这个环境变量，让它只作用于开发环境。这种方案解决了之前重复打包的问题，并且对代码的侵入行也很小。当你不想用这个方案的时候，只需要将这个插件移除就可以了。

   ```
   npm i babel-plugin-dynamic-import-node -D
   // .babelrc.js
   env: {
     development: {
     	plugins: ['dynamic-import-node']
     }
   }
   // router.js,正常懒加载方式书写即可
   const routes = [
   	{
   		path: '/home',
   		name: 'Home',
   		component: () => import('Home/index')
   	}
   ];
   ```

   
