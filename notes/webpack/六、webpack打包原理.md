#### 一、webpack启动过程

1. 从webpack命令行说起

通过`npm run scripts`运行webpack

```
// 开发环境
npm run dev

// 生产环境
npm run build
```

通过webpack直接运行

```
webpack entry.js bundle.js
```

在命令行运行以上命令后，npm会让命令行工具进入 `node_modules\.bin` 目录查找是否存在 `webpack.sh` 或者 `webpack.cmd` 文件，如果存在就执行，不存在就抛出错误。

实际的入口文件是：`node_modules\webpack\bin\webpack.js` 。
```
// webpack.js

// 正常执行时返回,异常时返回1
process.exitCode = 0;

// 运行某个命令
const runCommand = (command, args) => {...};

// 判断某个包是否安装
const isInstalled = packageName => {...};

// webpck可用的CLi:webpack-cli(全面)和webpack-command(轻量可配置)
const CLIs = [...];

// 判断两个cli是否安装了
const installedClis = CLIs.filter(cli => cli.installed);

// 根据安装数量进行处理
if (installed.length === 0) {...} else {...}
```

webpack最终找到webpack-cli(webpack-command)这个npm包，require进来，执行cli。

#### 二、webpack-cli

1. 引入yargs，对命令行进行定制
2. 分析命令行参数，对各个参数进行转换，组成编译配置项
3. 引用webpack，根据配置项进行编译和构建

#### 三、Tapable插件结果和Hooks设计

Tapable是一个类似EventEmitter的库，主要是控制钩子函数的发布与订阅，控制着webpack地插件系统。

#### 四、webpack流程

entry-option(初始化option) ——> run(开始编译) ——> make(从entry开始递归的分析依赖，对每个依赖模块进行build) ——> before-resolve(对模块位置进行解析) ——> build-module(开始构建某个模块) ——> normal-module-loader(将loader加载完成的module进行编译，生成AST树) ——> program(遍历AST，当遇到require等一些调用表达式时，收集依赖) ——> seal(所有依赖build完成，开始优化) ——> emit(输出到output目录)

#### 简单Demo

1. 可将ES6转换成ES5
 - 通过[babylon](https://www.npmjs.com/package/babylon)生成AST
 - 通过babel-core将AST重新生成源码
2. 可分析模块之间的依赖关系
 - 通过babel-traverse的ImportDeclaration方法获取依赖属性
3. 生产的JS文件可以在浏览器运行

webpack的模块机制
1. 打包出来的是一个IIFE(匿名闭包)
2. modules是一个数组，每一项是一个模块初始化函数
3. `_webpack_require`用来加载模块，返回`module.exports`
4. 通过`WEBPACK_REQUIRE_MOEHOD(0)`启动程序

打包之后的文件大致如下：
```
(function(modules) {
    var installedModules = {};
    function _webpack_require_(moduleId) {
        if (installedModules[moduleId]) 
            return installModules[moduleId].exports;
        var module = installed[moduleId] = {
            i: moduleId,
            l: false,
            exports: {}
        };
        modules[moduleId].call(module.exports, module, module.exports, _webpack_require_);
        module.l = true;
        return module.exports;
    }
    _webpack_require_(0);
})([
    /* 0 module */
    (function(module, _webpack_exports_, _webpack_require_) {...})
]);
```