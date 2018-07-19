# WePY_app

初步完成多页面跳转, 数据共享,
``` sh
npm install wepy-cli -g
npm  i && wepy build --watch
```

# 小程序项目

![扫一扫 使用微信小程序](./statics/gh_f498d6f2e269_344.jpg)

* nearby 基于高德地图api WePY 框架 周边生活导航 小程序
* dev 基于weui 框架 debug_app 的小程序版本

# 小程序开发问题

## 模块引入问题 及 数据更新问题

``` js
export default { request } // 定义模块 export default [ es6 语法 ]
import { request } from '../com/request' // 引入 request === undefined
import http from '../com/request' // 改为 http.request
module.exports = { request } // 改为 module.exports [ commonjs 语法 ]

if (resJson[url]) {
  isShowLoading && wepy.hideLoading && wepy.hideLoading()
  resolve(resJson[url])
  return 0 // Promise resolve 执行后 后面代码会继续执行
}

// 打开新页面
wepy.navigateTo({
  url: 'poi'
})
// 重定向
wepy.redirectTo({
  url: 'poi'
})
// 异步函数中更新数据的时，必须手动调用$apply方法 手动触发 脏数据检查
this.$apply()
```

## 组件使用问题

``` html
<!-- bindInput 定义为 async 异步方法 会导致 input 内容为 undefined -->
<input @input="bindInput" type="text" placeholder="搜索" focus="true" />
<!-- 定义css 仅在当前组件生效 -->
<style lang="less" scoped>
</style>
```

## 页面数据共享问题

``` js
// Gloabal 对象或 Storage 数据共享
wepy.setStorageSync(USER_INFO, obj)
wepy.getStorageSync(USER_INFO)
```

## 数据遍历 if else 判断

``` html
<view wx:for="{{checkboxItems}}" wx:key="value">
  <view wx:if="{{item.checked}}">{{item.name}}</view>
  <view wx:else>{{item.name}}</view>
</view>
```
## rpx可以根据屏幕宽度进行自适应

设备	rpx换算px (屏幕宽度/750)	px换算rpx (750/屏幕宽度)
iPhone5	1rpx = 0.42px	1px = 2.34rpx
iPhone6	1rpx = 0.5px	1px = 2rpx
iPhone6s	1rpx = 0.552px	1px = 1.81rpx
``` css
form {
  width: 100%;
}
```
## wx.showToast icon 只支持  success loading

``` js
// wx.showToast icon 只支持  success loading
// wepy.showToast image 定义 icon
wepy.showToast({
  title: str || '数据异常请重试',
  icon: 'error',
  image: '../images/error.png',
  duration: 1500
})
```

## 报错找不到模块问题

``` js
// issues 找不到crypto模块是因为你在代码中require了某些不能在小程序中使用的模块。
[Error] Error: 找不到模块: crypto
被依赖于: E:\jackdizhu\web\WePY_app\src\com\WXBizDataCrypt.js。
请尝试手动执行 npm install crypto 进行安装。
    at E:\jackdizhu\node\nvm\v8.9.0\node_modules\wepy-cli\lib\compile-script.js:98:31
    at String.replace (<anonymous>)
    at Object.resolveDeps (E:\jackdizhu\node\nvm\v8.9.0\node_modules\wepy-cli\lib\compile-script.js:46:21)
    at E:\jackdizhu\node\nvm\v8.9.0\node_modules\wepy-cli\lib\compile-script.js:270:27
    at <anonymous>
    at process._tickCallback (internal/process/next_tick.js:188:7)
    at Function.Module.runMain (module.js:678:11)
    at startup (bootstrap_node.js:187:16)
    at bootstrap_node.js:608:3
```

## 开发环境错误调试处理

``` js
// wepy.config.js
// 处理框架 大量 log 排错难问题
let log = console.log
console.log = (msg) => {
  let _arrR = [
    /\[编译\]/g,
    /\[写入\]/g,
    /\[拷贝\]/g
  ]
  for (let i = 0; i < _arrR.length; i++) {
    let r = _arrR[i].test(msg)
    if (r) {
      return 0
    }
  }
  log(msg)
}
```
