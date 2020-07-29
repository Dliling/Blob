#### 一、loader的链式调用和执行顺序

定义：loader只是一个导出为函数的JS模块。
```
module.exports = function(source) {
    return source;
};
```
多个loader串行执行，顺序从后到前。

#### 二、 [loader-runner](https://github.com/webpack/loader-runner)

定义：允许你在不安装`webpack`的情况下运行`loader`。

作用：
1. 作为webpack的依赖，webpack中使用它执行loader
2. 运行loader的开发和调试

使用：
```
import { runLoaders } from "loader-runner";

runLoaders({
	resource: "/abs/path/to/file.txt?query",

	loaders: ["/abs/path/to/loader.js?query"],

	context: { minimize: true },

	readResource: fs.readFile.bind(fs)

}, function(err, result) {});
```

#### 三、更复杂的loader的开发场景

一、loader的参数获取

通过[loader-utils](https://github.com/webpack/loader-utils)的getOptions方法获取

```
// 在loader中使用
const loaderUtils = require('loader-utils');
module.exports = function(content) {
    const {name} = loaderUtils.getOptions(this);
};
```

二、loader的异常处理

1. 直接通过throw抛出

2. 通过this.callback传递错误

```
this.callback(
    err: Error | null,
    content: string | Buffer,
    sourceMap?: SourceMap,
    meta?: any
);
```

三、loader的异步处理

通过`this.async`来返回一个异步函数,第一个参数是error，第二个参数是处理的结果。
```
module.exports = function(input) {
    const callback = this.async();
    // no callback -> return synchronous results
    // if (callback) {...}

    callback(null, input + input);
};
```

四、在loader中使用缓存

webpack中默认开启loader缓存，可以使用`this.cacheable(false)`关掉缓存

缓存条件：loader的结果在相同的输入下有确定的输出。有依赖的loader无法使用缓存

五、loader 如何进行文件输出？
通过`this.emitFile`进行文件写入
```
const loaderUtils = require('loader-utils');
module.exports = function(content) {
    // 替换占位符
    const {url} = loaderUtils.interpolateName(this, "[hash].[ext]", {
        content
    });

    this.emitFile(url, content);

    const path = `__webpack_public_path__${JSON.stringify(url)}`;
    return `export default${path}`;
};
```

关于自定义loader，可以移步[从零实现一个 Webpack Loader](https://blog.csdn.net/weixin_34121282/article/details/91471986)，文章讲解很详细~

#### 四、开发一个自动生成雪碧图的loader

```
background: url('a.png?__sprite')
background: url('b.png?__sprite')

=>

background: url('sprite.png')
```

如何将两张图合成一张图[spritemith](https://www.npmjs.com/package/spritesmith)
```
const Spritesmith = require('spritesmith');

const sprites = ['./imgs/a.jpg', './imgs/a.jpg'];
Spritesmith.run({
    src: sprites
}, function handleResult(err, result) {
    result.image,
    result.coordinates,
    result.properties
});
```

#### 五、插件基本结构介绍
插件只能在webpack中运行

I、插件的基本结构
```
// 插件名称
class MyPlugin {
    // apply方法
    apply(compiler) {
        // hooks
        compiler.hooks.done.tap('My Plugin', (
            // stats is passed as argument when done hooks is tapped
            stats
        ) => {
            // 处理逻辑
            console.log('hello world');
        })
    }
}

module.exports = MyPlugin;
```
II、插件的使用
```
plugins: [new MyPlugin()]
```

III、插件的错误处理

1. 参数校验阶段可以直接throw抛出
```
throw new Error('error');
```
2. 通过compilation对象的warnings和errors接收
```
compilation.warnings.push('warning);
compilation.errors.push('errors);
```

IV、通过compilation进行文件写入

compilation上的assets可以用于文件写入，可以将zip资源包设置到`compilation.assets`对象上

文件写入需要使用[webpack-sources](https://www.npmjs.com/package/webpack-sources)
```
const {RawSource} = require('webpack-sources');
class MyPlugin {
    constructor(options) {
        this.options = options;
    }

    apply(compiler) {
        const {name} = this.options;
        compiler.hooks.emit.tapAsync('MyPlugin', (compilation, cb) => {
            compilation.assets[name] = new RawSource('demo');
            cb();
        })
    }
}
```

#### 六、编写一个压缩构建资源为zip包的插件
要求：
1. 生成的zip包文件名称可以通过插件传入
2. 需要使用compiler对象上的特定hooks进行资源的生成

使用[jszip](https://www.npmjs.com/package/jszip)
```
var zip = new JSZip();
 
zip.file("Hello.txt", "Hello World\n");
 
var img = zip.folder("images");
img.file("smile.gif", imgData, {base64: true});
 
zip.generateAsync({type:"blob"}).then(function(content) {
    // see FileSaver.js
    saveAs(content, "example.zip");
});
```