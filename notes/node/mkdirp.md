https://www.npmjs.com/package/mkdirp

#### 作用

递归创建目录，即使上层目录不存在，也会逐级创建

#### 用法

```
// 1.0.0版本以上
mkdirp(path)
	.catch(err => {
		console.log(err);
	})
	.then(() => {
		// writeFile的文件名不能有:、空格等
		// 可以自动创建文件，但是只能是当前路径，不能自动创建目录文件夹
		fs.writeFile(path, 'test', err => {
			if (err) {
				throw err;
			}
		});
	});
// 1.0.0版本以下
mkdirp(path, {opts}, err => {
	if (err) {
		throw err;
	}
});
```

具体的配置就不再详说，主要是注意版本的差别，否则会报错。

若1.0.0版本以上，依旧使用传入cb的方式，会报错。

```
invalid options argument
```

