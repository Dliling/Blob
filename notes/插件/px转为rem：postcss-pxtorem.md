#### 简介

PostCSS的插件，用于将像素`px`生成`rem`单位。

#### 使用

```
npm i postcss-pxtorem -D
// postcss.config.js
module.exports = {
	plugins: {
		autoprefixer: {},
		'postcss-pxtorem': {
			rootValue({file}) {
				return file.indexOf('vant') !== -1 ? 37.5 : 75;
			},
			propList: ['*']
		}
	}
}
```

#### 参数详解

- **rootValue**：(Number|Function)，表示根元素大小，或者根据函数传入的参数返回根元素字体大小。
- **propList**：(Array)，可以从`px`更改为`rem`的属性。
  - 值必须完全匹配。
  - 使用通配符*****表示所有属性都需要转换。
  - 使用 **!** 表示不匹配的属性。例如：`['*', '!letter-spacing']`
  - ***** 在单词的开头或结尾使用。例如：`['*position*']`，将匹配`background-position-y`
  - **!** 和 ***** 可以结合使用
- **selectorBlackList**：(Array)，不需要转换为`rem`的属性。
  - 如果为字符串，则检查选择器是否包含字符串。例如：`['demo']`将匹配`.demo-title`
  - 如果是正则表达式，则检查选择器是否匹配正则表达式
- **minPixelValue**：(Number)，设置要替换的最小像素值。
- **exclude**：(String|RegExp|Function)，需要保留`px`的文件路径
  - 如果为字符串，则检查文件路径是否包含字符串。
  - 如果为正则，则检查文件路径是否匹配正则。
  - 如果为函数，回调函数将文件路径作为参数，返回值为`true`时，则该文件被忽略。
- **mediaQuery**：(Boolean)，是否允许在媒体查询中转换`px`。
- **unitPrecision**：(Number)，转换为`rem`后的精确度，即保留几位小数。

#### 忽略转换

书写单位时使用**大写字母**。

````
border-width: 1Px;
border-width: 1PX;
````

