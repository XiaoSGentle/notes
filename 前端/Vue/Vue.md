1. 新建vite项目
```js
npm create vite@latest 你的项目名  -- --template vue
```
2. 安装unocss
```js
npm i -D unocss
```
3. 下载unocss的依赖
```js
pnpm i -D unocss @unocss/preset-uno @unocss/preset-attributify @unocss/preset-icons
```
4. 配置vite.config.js
```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
// 引入Unocss
import Unocss from 'unocss/vite';
import { presetUno, presetAttributify, presetIcons } from 'unocss'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    Unocss({ // 使用Unocss
      presets: [
        presetUno(),
        presetAttributify(),
        presetIcons()
        ],
    })
  ]
})
```
5. 在main.js中引入unocss
```js
import 'uno.css'
```
6. 参考https://www.tailwindcss.cn/docs进行开发

**如果你很懒，请省略所有以上的步骤，直接用那谁的框架**
```shell
npx degit antfu/vitesse-lite 你的项目名
```
# 镜像的切换
* 查看源
```js
npm get registry
yarn config get registry
pnpm get registry
```
* 切换源(taobao镜像)
```js
npm config set registry https://registry.npm.taobao.org
yarn config set registry https://registry.npm.taobao.org/
pnpm config set registry https://registry.npm.taobao.org
//pnpm还原
pnpm config set registry https://registry.npmjs.org
```
# npm、yarn、pnpm 互相切换
## npm、yarn换pnpm
安装pnpm为全局包
```js
npm i -g pnpm@latest
```
移除npm依赖项，安装库，锁文件
移除yarn的依赖项，安装库，锁文件
```js
rm node_modules package-lock.json

rm node_modules yarn.lock
```
使用pnpm安装依赖
```js
pnpm i
```
清理冗余
```js
pnpm prune
```

##  pnpm换npm
清除依赖文件
```js
rm node_modules pnpm-lock.yaml
```
使用npm安装依赖
```js
npm i
```

# 命令的启动
```js
npm(pnpm) run dev
Ctrl+C : 停止命令
```
# 端口修改
## 临时指定端口(其他同理)
```js
npm run serve -- --port 8081
port=5000 npm run serve

```
## vue.config.js中永久配置
```js
const {defineConfig} = require('@vue/cli-service')
module.exports = defineConfig({
    transpileDependencies: true,
    //更改默认端口
    devServer: {
        open: false, // 自动打开浏览器
        port: 8081,
    },
    //设置是否在开发环境下每次保存代码时都启用 eslint验证
    lintOnSave: false
})

```
# Vue中文件分析
-   assets 是资源文件夹，通常我们会把图片资源放在里面。
-   components 文件夹通常会放一些组件。
	- 可重用性文件🦶foot ，🔝top 
-   router 文件夹里面放的是 VueRouter 的相关配置。
	- router/index.js 文件，我们可以看到路由配置信息
-   store 文件夹里面放的是 Vuex 的相关配置。
-   views 文件夹里面通常放置页面的 .vue 文件。
	- 每个页面文件
-   App.vue 定义了一个根组件。
-   main.js 是项目的入口文件。
	- 这个文件中引入一些app组件、VueRouter，配置router，Vuex配置store
	- 通过 new Vue () 创建 Vue 实例，并将 router、store 配置传入。通过 render 函数渲染组件 App。并将 Vue 实例挂载到 id 为 app 的 div 上。

# Element UI
## 导入
```
npm i element-ui -S
npm install element-plus --save
```
style  scoped 组件样式只在当当前组件生效
在router.js中可以重定向操作 { }