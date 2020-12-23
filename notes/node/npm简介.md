### 简介

npm 是 Node.js 标准的软件包管理器。npm 是随着 node 一起安装的，常用的场景如下：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

新版本的 node 已经集成了 npm ，所以之前npm也一并安装好了。同样可以通过输入 **"npm -v"** 来测试是否成功安装。命令如下，出现版本提示表示安装成功:

```
npm -v
```

若是旧版本的 npm，可以通过重新安装来升级：

```
npm i npm -g
```

### 下载

npm 可以管理项目依赖的下载。

#### 安装所有依赖

如果项目中有 package.json 文件，则通过运行：

```
npm install
```

它会在 node_modules 文件夹（如果不存在则会创建）中安装项目所需的所有东西。

#### 安装单个软件包

也可以通过运行以下命令安装特定的软件包：

```
npm install <package-name>
```

通常会在此命令中看到更多标志：

- --save ：安装并添加条目到 package.json 文件的 dependencies。
- --save-dev 或 -D：安装并添加条目到 package.json 文件的 devDependencies。
- --production：只安装 package.json 文件的 dependencies 下的软件包，可能 download 这个库只是为了打包。

区别主要是，devDependencies 通常是开发的工具（例如测试的库），而 dependencies 则是与生产环境中的应用程序相关。

安装软件包的指定版本：

```
npm install <package-name>@<version>
```

可以通过以下命令查看软件包的所有版本：

```
npm view <package-name> versions
```

#### npm 将软件包安装到哪里

当使用 npm 安装软件包时，可以执行两种安装类型：

- 本地安装
- 全局安装

默认情况下，当输入 npm install 命令时，例如：

```
npm install lodash
```

软件包会被安装到当前文件树中的 node_modules 子文件夹下。

在这种情况下，npm 还会在当前文件夹中存在的 package.json 文件的 dependencies 属性中添加 lodash 条目。

使用 -g 标志可以执行全局安装：

```
npm install -g lodash
```

在这种情况下，npm 不会将软件包安装到本地文件夹下，而是使用全局的位置。

全局的位置到底在哪？

npm root -g 命令会告知其在计算机上的确切位置。在 macOS 或 Linux 上，此位置可能是 / usr / local / lib / node_modules 。在Windows 上，可能是 C: \ Users \ YOU \ AppData \Roaming \ npm \ node_modules 。

但是，如果使用 nvm 管理 Node.js 版本，则该位置会有所不同。例如，使用 nvm ，则软件包的位置可能为：/ Users / joe / .nvm / versions / node / v14.7.0 / lib / node_modules 。

#### 更新软件包

如何查看是否有新版本呢？

```
npm outdated
```

<img src="/Users/liling/Library/Application Support/typora-user-images/image-20201223180512313.png" alt="image-20201223180512313" style="zoom:50%;" />

我们看到版本号是三位的，X. Y. Z。

- X：主版本。当进行不兼容的API更改时，则升级主版本。
- Y：次版本。当以向后兼容的方式添加功能时，则升级次版本。
- Z：补丁版本。当进行向后兼容的缺陷修复时，则升级补丁版本。

这个版本号规则，每个npm软件包都必须遵守，因为整个系统都依赖于此。

如果有新的次版本或补丁版本，通过运行以下命令，更新也变得容易：package.json不变，而package-lock.json文件会被新版本填充。

```
npm update
```

上图eslint有一个主版本的发布，但是运行npm update不会更新主版本。主版本永远不会以这种方式更新，因为主版本发布意味着有重大修改，而npm希望为你减少麻烦。

若要强制更新主版本，则可以全局安装npm-check-updates：

```
npm install npm-check-updates

// 然后运行
ncu -u
```

这会升级package.json中的dependencies和devDependencies中的所有版本，以便npm可以安装新的主版本。现在可以运行更新了：

```
npm update
```

npm 会检查所有软件包是否有满足版本限制的更新版本。

也可以指定单个软件包进行更新：

```
npm update <package-name>
```

#### **卸载软件包**

```
npm uninstall <package-name>
```

卸载后，可以到 node_modules 文件夹下查看是否卸载成功，或者使用以下命令查看：

```
npm list
```

结果示例如下：

<img src="/Users/liling/Library/Application Support/typora-user-images/image-20201223171945081.png" alt="image-20201223171945081" style="zoom:50%;" />

若仅需要查看顶层的软件包（即在package.json中列出的），则运行：

```
npm list --depth=0
```

结果示例如下：

<img src="/Users/liling/Library/Application Support/typora-user-images/image-20201223172323139.png" alt="image-20201223172323139" style="zoom:50%;" />

也可以查看全局安装的顶层的软件包：

```
npm list -g --depth=0
```

![image-20201223174215797](/Users/liling/Library/Application Support/typora-user-images/image-20201223174215797.png)

也可以通过指定软件包的名字，来获取软件包的版本

```
npm list vue
```

<img src="/Users/liling/Library/Application Support/typora-user-images/image-20201223173537670.png" alt="image-20201223173537670" style="zoom:150%;" />

这个也适用于安装的软件包的依赖：

```
npm list semver
```

<img src="/Users/liling/Library/Application Support/typora-user-images/image-20201223174006645.png" alt="image-20201223174006645" style="zoom:50%;" />

#### **搜索软件包**

```
npm search <package-name>
```

### 版本控制

除了简单的下载外，npm 还可以管理版本控制，因此可以指定软件包的任何特定版本，或者要求版本高于或低于所需版本。

很多时候，一个库仅与另一个库的主版本兼容。或者，一个库的最新版本中有一个缺陷（仍未修复）引起了问题。

指定库的显式版本还有助于使每个人都使用相同的软件包版本，以便整个团队运行相同的版本，直至 package.json 文件被更新。

在所有这些情况中，版本控制都有很大的帮助，npm 遵循语义版本控制标准。

### 运行任务

package.json 文件支持一种用于指定命令行任务（可通过使用以下方式进行）的格式：

```
npm run <task-name>
```

例如：

```
{
	"scripts": {
		"start": "node lib/server-production"
	}
}
```

可以运行如下，而不是输入那些容易忘记或输入错误的长命令：

```
npm run start
```

