package.json是项目的清单。它必须是严格的JSON格式，而不是JS对象。

### 基本字段

##### name

如果你需要发布你的包，name和version是必要的。name和version一起组成的标识假设是唯一的，改变包的同时应该改变version。如果你的包不需要发布，name和version是可配置的，即不是必须的。结合private：true使用，那么npm会拒绝发布这个包。

name的注意事项如下：

- 名称必须少于214个字符，不允许包含空格和大写字母，以点或下划线开头。
- 这个名字会作为在URL的一部分、命令行的参数或者文件夹的名字。任何non-url-safe的字符都是不能用的。
- 在发布这个名字之前，需要去npm registry看下是否有重名。
- 不要把node或js放在你的名字中，因为你写了package.json，它被认为是一个js文件，不过你可以用"engine"字段指定一个引擎（见后文）。
- 这个名字可能会作为参数被传入require()，所以它应该比较短，但也要意义清晰。

##### version

表明包的当前版本。如果你需要发布你的包，name和version是必要的。name和version一起组成的标识假设是唯一的，改变包的同时应该改变version。如果你的包不需要发布，name和version是可配置的，即不是必须的。结合private：true使用，那么npm会拒绝发布这个。

version必须能够被server解析，它被包含在npm的依赖中。如果要自己使用，npm install server。

可用的数字和范围见[semver](https://www.npmjs.cn/misc/semver/)

### 信息类字段

##### description

对包的简短描述。是字符串。方便人们可以检索到软件包，因为它被列在npm search中。

##### keywords

关键字，是一个字符串数组。方便人们可以检索到软件包，因为它被列在npm search中。

##### author

作者信息，可以是字符串，也可以是对象。person是一个有name属性的对象，email和url字段可选。

```
{
	"name": "aaa",
	"email": "a@163.com",
	"url": "http://aa.com"
}
```

或者可以写一个字符串，npm会解析。

```
aaa <a@163.com> （http://aa.com）
```

或者也可以在你的npm用户群中设置一个顶级字段：maintainers。

##### contributors

贡献者信息，是一个数组。和author字段中的person对象一样。

##### repository

指明项目的代码仓库。对于想要贡献代码的人很有帮助。如果在github仓库，可以通过npm docs命令可以找到你。

```
"repository": {
	"type": "git",
	"url": "https://github.com/aaa/npm.git"
}
```

type可以是git或svn。URL应该是公开的，即便是只读的，能直接被未经过修改的版本控制程序处理的url。不应该是一个html的项目页面。因为它是给计算机看的。

如果你的package.json没在项目根目录下，你可以通过directory字段来指定它在哪。

```
"repository": {
	"type": "git",
	"url": "https://github.com/aaa/npm.git",
	"directory": "packages/react-dom"
}
```

##### homepage

项目官网的URL。

```
"homepage": "https://github.com/owner/project#readme"	
```

##### bugs

项目提交问题的地址或邮箱。这对于遇到问题的人很有帮助。

```
{ "url" : "https://github.com/owner/project/issues",
	"email" : "project@hostname.com"
}
```

你可以指定一个或者指定两个。如果你只想提供一个url，那就不用对象了，字符串就行。

如果提供了url，它会被`npm bugs`命令使用。

##### license

你应该提高一个许可证，让人们知道使用的权利和限制。

假如你使用的是通用的BSD或MIT等许可证，就只需要指定一个名字。

```
{
	"license": "MIT"
}
```

如果你不希望赋予其他人在任何条款下使用私有或未发布包的权利：

```
"license": "UNLICENSED"
```

考虑结合private: true一同使用，以此防止意外发布。





##### scripts

是一个脚本命令组成的集合，这些命令在包不同的生命周期被执行。key是生命周期时间，value是需要运行的命令。

详见[scripts](https://docs.npmjs.com/cli/v7/using-npm/scripts)。



### 依赖类

##### dependencies

依赖关系是在一个简单对象中指定的，这个对象将包名映射到一个版本范围。版本范围是具有一个或多个空格分隔的描述符的字符串。依赖关系也可以用tarball或git URL来标识。

请不要将测试或过渡性的依赖放在dependencies中。

版本号：X. Y. Z，主版本 . 次版本 . 补丁版本。先看下版本范围的规则：

- version：必须和version完全一致。
- \>version：要大于version。
- \>=version：要大于等于version。
- \<version：小于version。
- \<=version：小于等于version。
- \~version：大约一致，只能更新补丁版本。
- ^version：只能更新补丁版本和次版本。
- `1.2.x`： 1.2.0, 1.2.1, 等等，但不是 1.3.0
- http://...：见下方依赖URLs。
- *：匹配任何版本。
- ""：空，和 * 意思一致。
- version1 - version2：类似 >= version1  =< version2
- range1 || range2 ：range1和range2二选一。
- git...：见下方的依赖GIT URLs。
- user / repo：见下方的Github URLs。
- tag：标记并作为标记发布的tag。
- /local/local：见下方的本地路径。
- latest：使用可用的最新版本。

```
{ "dependencies" :
  { "foo" : "1.0.0 - 2.9999.9999",
  	"bar" : ">=1.0.2 <2.1.2",
  	"baz" : ">1.0.2 <=2.3.4",
  	"boo" : "2.0.1",
  	"qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0",
  	"asd" : "http://asdf.com/asdf.tar.gz",
  	"til" : "~1.2",
  	"elf" : "~1.2.3",
  	"two" : "2.x",
  	"thr" : "3.3.x",
  	"lat" : "latest",
  	"dyl" : "file:../dyl"
  }
}
```

###### 依赖URLs

你可以指定一个tarball URL，这个tarball将在包初始化的时候下载并初始化。

###### 依赖Git URLs

格式如下：

```
<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]
```

protocol可以是以下任意一个：git`, `git+ssh`, `git+http`, `git+https`, git+file

如果提供了#<commit-ish>，它将被用来完全克隆该提交。如果提交的格式为#semver:<semver>， <semver>可以是任何有效的semver范围或精确版本，npm会在远程存储库中查找任何匹配该范围的标签或引用，就像它在注册表依赖中所做的那样。如果#<commit-ish>和#semver:<semver>都没有指定，那么使用master。

```
git+ssh://git@github.com:npm/cli.git#v1.0.27
git+ssh://git@github.com:npm/cli#semver:^5.0
git+https://isaacs@github.com/npm/cli.git
git://github.com/npm/cli.git#v1.0.27
```

###### Github URLs

1.1.65版后，你可以仅仅用“user/foo-project”引用GitHub urls，与Git URLs一样，可以包含提交后缀。比如：

```
{
  "name": "foo",
  "version": "0.0.0",
  "dependencies": {
    "express": "expressjs/express",
    "mocha": "mochajs/mocha#4727d357ea",
    "module": "user/repo#feature\/branch"
  }
}
```

###### 本地路径

从2.0.0版本开始，可以提供到包含包的本地目录的路径。本地路径可以通过以下任何形式保存:

```
../foo/bar
~/foo/bar
./foo/bar
/foo/bar
```

在这种情况下，它们将被规范成一个相对路径被添加到package.json。

```
{
  "name": "baz",
  "dependencies": {
    "bar": "file:../foo/bar"
  }
}
```

这个特性对于本地脱机开发和创建测试很有帮助，这些测试需要安装npm，而你不想要用到外部服务器，但在将包发布到公共注册表时不应该使用。

##### devDependencies

如果有人要使用你的模块，那么他们可能不需要你开发使用的外部测试或者文档框架。

在这种情况下，最好将这些附属的项目列在`devDependencies`中。

这些东西会在执行`npm link`或者`npm install`的时候初始化，并可以像其他npm配置参数一样管理。



### 平台类

##### engines

指定工作的node的版本。

```
"engines": {
	{
		"node" : ">=0.10.3 <0.12"
	}
}
```

如果省略了"engines"，那么npm就会假定它在node上工作。

##### os

指定你的模块运行在哪个操作系统上。

```
"os": ["linux"]

// 阻止某个操作系统
"os": ["!win32"]
```

主机操作系统由process.platform决定。

##### cpu

如果你的代码只在指定的CPU架构上运行，你可以指定：

```
"cpu": ["x64"]
// 阻止某个cpu
"cpu": ["!arm"]
```

主机cpu架构由process.arch决定。

##### browerslist

用于告知要支持哪些浏览器（及其版本）。 Babel、Autoprefixer 和其他工具会用到它，以将所需的 polyfill 和 fallback 添加到目标浏览器。

```
"browserslist": [
  "> 1%",
  "last 2 versions",
  "not ie <= 8"
]
```



### 发布类

##### private

如果设置为true，则表明拒绝发布。

这是一个防止私库意外发布的方法。如果你要确定给定的包是只发布在特定registry（如内部registry）的，用`publishConfig`hash的描述来重写`registry`的publish-time配置参数。

##### publishConfig

这是一个在publish-time使用的配置集合。如果您想要设置标记、注册表或访问，这样就可以确保给定的包没有被标记为“latest”、发布到全局公共注册表或指定范围的模块在默认情况下是私有的，那么它就特别方便。

任何配置值都可以被覆盖，但只有“标签”、“注册表”和“访问”可能对发布的目的有影响。

请参阅config查看可覆盖的配置选项列表。



### 文件类

##### files

files是一个包含项目中的文件的数组。如果命名了一个文件夹，那也会包含文件夹中的文件。（除非被其他条件忽略了）

你还可以在包的根目录或子目录下提供一个".npmignore"文件来忽略项目包含文件，即使这些文件被包含在files字段中。.npmignore文件和.gitignore的功能很像。

某些文件总是被包含的，不论是否在规则中指定了它们：package.json、README (and its variants)、CHANGELOG (and its variants)、LICENSE / LICENCE、NOTICE。

相反地，一些文件总是被忽略：

.git、CVS、.svn、.hg、.lock-wscript、.wafpickle-N、.swp、.DS_Store、._*、npm-debug.log

##### main

配置一个文件名指向模块的入口程序。如果你的包名为foo，那么使用者使用时require('foo')，main配置的模块exports对象会被返回。这应该是一个相对于项目根目录的文件路径。

对大多数模块而言，最重要的是有一个主脚本，其他就没什么了。

##### directories

CommonJS包规范详细介绍了几种使用directories对象指示包结构的方法。如果你看看npm的package.json，您将看到它有doc、lib和man的目录。

在未来，这些信息可能会被用于其他创造性的方式。

###### directories.lib

告诉别人你的库文件夹在哪里。目前没有什么特别的东西需要用到lib文件夹，但确实是重要的元信息。

###### directories.bin

如果你指定一个“bin”目录，然后在那个文件夹中得所有文件都会被当做"bin"字段使用。

由于bin指令的工作方式，同时指定bin路径和设置directories.bin是错误的。如果要指定单个文件，请使用bin，对于现有bin目录中的所有文件，请使用directories.bin。

###### directories.man

一个放满man页面的文件夹，贴心地放一个man字段。

###### directories.doc

这里放的是markdown文件，也许有一天会被很好地展示出来。

###### directories.example

将示例脚本放在这里，某一天，他可能会以某种巧妙的方式暴漏出来。

###### directories.test

将你的测试放在这里，它现在不会被暴露出来，某一天也许会。

##### bin

许多包都有一个或多个可执行文件希望被放到环境变量PATH中。npm让这些so easy。事实上，正是这个功能使npm是可执行的。

要用这个功能，给package.json中的`bin`字段一个命令名到文件位置的map。初始化的时候npm会将他链接到`prefix/bin`（全局初始化）或者`./node_modules/.bin/`（本地初始化）。

```
{ "bin" : { "myapp" : "./cli.js" } }
```

当你安装myapp时，它会创建一个符号链接：从cli.js`脚本到`/usr/local/bin/npm。

如果你的包里只有一个可执行文件，并且文件名和包名一致，可以使用字符串：

```
{
	"name": "my-program",
	"version": "1.2.5",
	"bin": "./path/to/program"
}
```

请确保您在bin中引用的文件以#!/usr/bin/env开头，否则会启动没有节点可执行文件的脚本!

##### browser

如果你的模块是客户端使用的，那么应该使用browser字段而不是main字段。这有助于提示用户它可能依赖于Node.js模块中不可用的原因。如window。



### 任务类

##### 命令特有的属性

可以承载命令特有的配置，如ESlint，Babel。

每个都有特有的属性，例如 `eslintConfig`、`babel` 等。 它们是命令特有的，可以在相应的命令/项目文档中找到如何使用它们。

##### config

"config"对象可以用来配置包脚本中的跨版本参数。例如：

```
{
	"name": "foo",
	"config": {
		"port": "8000",
	}
}
```

然后有一个start命令，引用了npm_package_config_port环境变量，用户可以通过npm  config set foo:port 8001来覆写。





