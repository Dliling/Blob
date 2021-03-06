当修改 *node_modules* 树或者 *package.json* 时，*package-lock.json* 会自动生成。它描述了生成的确切树，以便后续安装能生成相同的树，而不去管依赖包中间的变更版本。

这个文件应该被提交到代码源库中，有以下几个用处：

- 描述依赖关系树的单一表示，以保证团队成员、部署和持续集成安装完全相同的依赖关系。
- 为用户提供一种工具，让用户“穿越”到 *node_modules* 的以前状态，而不必提交目录本身。
- 通过可读的源代码控制差异来促进树变化的可视性。

关于 *package-lock.json* 的一个关键细节，那就是它不能被发布。如果你在项目根目录以为的地方找到这个文件，它都会被忽略。

当某人尝试通过 *npm install* 初始化项目时，下载的依赖包可能和项目初期是不同的，尤其是 *package.json* 中的版本没有固定，可以更新时，虽然补丁版本或次版本不应该引起重大改变，但还是有概率引入 *bug*。

*package-lock.json* 会固化当前安装的每个依赖包的版本，每当 *npm install* 时，*npm* 会使用这些确切的版本。

![WX20210104-173501@2x](/Users/liling/Downloads/WX20210104-173501@2x.png)

上图是我从 package-lock.json 中截取的一部分，下面来解读一下图中这些字段：

- *version* : 软件包的版本号。
- *resolved* : 指向软件包位置。 
- *integrity* : 校验软件包。
- *dev* : 在项目中是否是开发依赖。
- *requries* : 需要的其他软件包。
- *dependencies* : 该包的依赖包。