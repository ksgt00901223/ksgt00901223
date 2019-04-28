---
classify: 工具插件
tags: [VSCode, 工具]
---

## VSCode 的一些常用的代码模板

### 自定义模板的步骤

进入 vscode 的设置，然后选择用户自定义代码段，然后命名即可。

### vue 模板

vue 的基础模板，一些插件也包含 vue 的插件，但是内容不是特别多，这个默认光标是给组件命名类名，tab 键切换为为组件写注释，如用途是什么，将下面代码直接复制到最外层的对象里面即可
触犯指令为 vue

```
"vue": {
		"prefix": "vue",
		"body": [
				"<!-- $2 -->",
				"<template>",
				"  <div class='$1'>\n",
				"  </div>",
				"</template>\n",
				"<script type=\"text/ecmascript-6\">",
				"export default {",
				"    components: {\n",
				"    },",
				"    props: {\n",
				"    },",
				"    data() {",
				"        return {\n",
				"        }",
				"    },",
				"    methods: {\n",
				"    },",
				"}",
				"</script>\n",
				"<style scoped lang=\"scss\">",
				".$1{\n",
				"}",
				"</style>",
				""


		],
		"description": "vue模板"
```

### axios 的请求部分

在组件页面

```
import {xxx} from '@/api'
```

然后再 motheds 里面
写函数名，然后 aw 按 tab，aw 根据实际情况修改，此例子里面用了饿了么 ui 的消息提示，可按需删除，此外，此操作为返回 code 为 0，需按项目修改

```
"aw": {
    "prefix": "aw",
    "body": [
      "$1(params).then(res=>{",
      "  if(parseInt(res.code) === 0){\n",
      "  }else {",
      "  this.$$message.error(res.msg)",
      "}",
      "}).catch(err => {",
      "  console.log(err)",
      "})"
    ],
    "description": "ajax模板"
  }
```
