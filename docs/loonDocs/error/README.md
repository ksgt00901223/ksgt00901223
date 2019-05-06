---
title: 错误集合
date: 2019/05/04
---
vue
react
插件
软件
运行
系统


### mac 权限问题，每次都要 sudo npm

**描述：** 每次运行 npm 时都要加上 sudo，然后还要输密码。
**解决：** 为当前账户添加 node_modules 目录读写权限。

```sh
sudo chown -R $(whoami) ~/.npm
```

### 打包时出现 localStorage 未定义等

**描述：** 打包时出现 localStorage、sessionStorage 等未定义报错。
**解决：** 看运行环境是否包含 localStorage、sessionStorage，比如是否要加 window 对象，或者 ssr 打包时生命周期开始没有浏览器对象 localStorage、sessionStorage，所以会报错。需放到 mounted 钩子里。

### vue 组件热更新无效

**描述：** vue 组件热更新无效，需重启才行。
**解决：** 查看组件引用路径是否正确，尤其大小写。

### vue 的 this.\$router.go(0)在 safari 上无效

**描述：** vue 的 this.$router.go(0)在safari上无效。
**解决：** 用this.$router.push()或者用原生的 history.go(-1) ，location.reload() 。
