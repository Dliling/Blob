[eslint](https://eslint.bootcss.com/)：代码风格检查工具，配置在`.eslintrc.js`或者`package.json`中的`eslintConfig`

[husky](https://www.npmjs.com/package/husky)：git hook工具，阻止不好的git提交

[lint-staged](https://www.npmjs.com/package/lint-staged)：参考Git概念的暂存区，即`git add`之后文件就到了暂存区，即待提交

[commitizen](https://www.npmjs.com/package/commitizen)：编写合格的commit信息，较友好的提交信息格式可参考[Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)，全局安装

cz-conventional-changelog

validate-commit-msg

以上的安装就不再说了，主要说下配置～

```
// package.json

{
	"scripts": {
		"lint": "eslint --ext .js",
		"commit": "git-cz"
	},
	"eslintConfig": {
		"root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "babel-eslint"
    },
    "rules": {
    	"indent": 2
    }
	},
	"husky": {
		"hooks": {
			"pre-commit": "lint-staged"
		}
	},
	"lint-staged": {
		"src/*.js": ["eslint --fix", "git add"]
	}
}
```

以上配置在提交前，检查并修复**本次提交文件**的代码风格，若通过，则继续下一步，若不通过，则退出。若不使用`lint-staged`，检查所有文件，很容易发生灾难～也可以使用代码格式化工具[prettier](https://prettier.io/)，也可以参考[prettier中文文档](https://www.kancloud.cn/luponu/prettierjs/872223)

到这里，对于自觉的人来说，提交已经可以了，但是在选择提交信息时，有人不遵守规则，怎么办呢？

这时就需要[@commitlint/config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional)来检测提交信息是否符合规范了～

继续在`package.json`中配置

```
// package.json
{
  "husky": {
      "hooks": {
        "pre-commit": "lint-staged",
        "commit-msg": "validate-commit-msg"
      }
    },
    "config": {
			"commitizen": {
				"path": "./node_modules/cz-conventional-changelog"
			}
    }
  }
```

我写的比较简洁，多是配置，详细的可参考[commitizen + husky 规范git提交信息](https://www.codenong.com/j5dff4220518825121e3/)

