官方文档<br />[API 参考 | Vue.js](https://cn.vuejs.org/api/)
<a name="skyvb"></a>
## VUE3 API 整体盘点

- **VUE3 API主要包含以下六个部分**
   - **全局API ----  **全局会用到的API
   - **组合式API ---- ** vue3所拥有的组合式API
   - **选项式API ---- **  vue2所拥有的选项式API
   - **内置内容 ---- **  指令、组件、特殊元素和特殊属性
   - **单文件内容 ---- ** 语法定义、
   - **进阶API ---- **渲染函数、服务端渲染、TS工具类型和自定义渲染

![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1702910211146-0318382d-43c9-44ae-9a64-8897ef9c1a06.webp#averageHue=%23efc56f&clientId=u3fa0f967-cd9f-4&from=paste&id=ufa8a0131&originHeight=885&originWidth=832&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ucaf3c487-65c3-4ae2-aedc-08680c33580&title=)
<a name="LElIk"></a>
## 全局API
Vue3的全局API包含两个部分：应用实例和通用API。那它们各自都有哪些内容呢？
<a name="dusgN"></a>
### 应用实例
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1702910261926-b624a96e-4c91-4883-96c4-833f4ecc8470.webp#averageHue=%23fbf9f8&clientId=u3fa0f967-cd9f-4&from=paste&id=u6bacf7b8&originHeight=2548&originWidth=2773&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf17f8f8c-62c4-45dc-bfdd-d575b676bb9&title=)
<a name="kqE1Y"></a>
#### 方法
```vue
import { createApp } from 'vue';
import App from './App.vue';
  
const app = createApp(App)
console.log("vue版本号：",app.version);
console.log("vue配置：",app.config);

```

- **app.version方法： 返回值为当前VUE版本号，主要用于第三方插件的使用判断版本是否兼容。**
- **app.config方法：每一个应用实例都会暴露一个config对象，这个对象包含了对这个应用的配置。**
   - **什么时候更改这些属性？：在挂载应用前可以进行更改**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1703403473752-f2b9d3f4-59bf-45b5-b7bd-642353c21a1b.png#averageHue=%23fcfcfb&clientId=u413b99a4-92ec-4&from=paste&height=511&id=u08bc01f4&originHeight=511&originWidth=1082&originalType=binary&ratio=1&rotation=0&showTitle=false&size=56854&status=done&style=none&taskId=u332caa4d-ad52-4c7e-9615-6627f76a278&title=&width=1082)

- **app.config.errorHandler : 用于为应用内抛出一个未捕获错误指定一个全局处理函数**
   - 错误处理器接收**三个参数**：错误对象、触发该错误的组件实例，一个指出错误来源类型信息的字符串。
   - 它可以从下面这些来源中捕获错误：
      - 组件渲染器
      - 事件处理器
      - 生命周期钩子
      - setup() 函数
      - 侦听器
      - 自定义指令钩子
      - 过渡 (Transition) 钩子
```javascript
app.config.errorHandler = (err, instance, info) => {
  // 处理错误，例如：报告给一个服务
}
```

- **app.config.warnHandler : 用于对vue在运行是所弹出的警告，来指定一个自定义处理函数**
   - 注意：警告仅会在开发阶段显示，因此在生产环境中，这条配置将被忽略。
```javascript
app.config.warnHandler = (msg, instance, trace) => {
  // `trace` is the component hierarchy trace
}
```

- **app.config.performance : 当设置为true时，可以在浏览器开发工具的性能/时间线页面中启用对组件初始化，编译，渲染，修补的性能表现进行追踪**
- **app.config.compilerOptions : **
   - **app.config.compilerOptions.isCustomElement **用于指定一个检查方法来识别原生自定义元素。
   - **app.config.compilerOptions.whitespace** 用于调整模板中空格的处理行为。
   - **app.config.compilerOptions.delimiters** 用于调整模板内文本插值的分隔符。
   - **app.config.compilerOptions.comments** 用于调整是否移除模板中的 HTML 注释
```javascript
// 将所有标签前缀为 `ion-` 的标签视为自定义元素
app.config.compilerOptions.isCustomElement = (tag) => {
  return tag.startsWith('ion-')
}

//  'condense' | 'preserve'
app.config.compilerOptions.whitespace = 'preserve'

// 分隔符改为ES6模板字符串样式
app.config.compilerOptions.delimiters = ['${', '}']

app.config.compilerOptions.comments = true
```

- **app.config.globalProperties : **一个用于注册能够被应用内所有组件实例访问到的全局属性的对象。
   - 这是对 Vue 2 中Vue.prototype使用方式的一种替代，此写法在 Vue 3 已经不存在了。与任何全局的东西一样，应该谨慎使用。
```javascript
//全局注册
app.config.globalProperties.msg = 'hello'

--------------------

// 这使得msg在应用的任意组件模板上都可用，并且也可以通过任意组件实例的this访问到：
export default {
  mounted() {
    console.log(this.msg) // 'hello'
  }
}

```

- **app.config.optionMergeStrategies ：**一个用于定义自定义组件选项的合并策略的对象
```javascript
const app = createApp({
  // option from self
  msg: 'Vue',
  // option from a mixin
  mixins: [
    {
      msg: 'Hello '
    }
  ],
  mounted() {
    // 在 this.$options 上暴露被合并的选项
    console.log(this.$options.msg)
  }
})

// 为  `msg` 定义一个合并策略函数
app.config.optionMergeStrategies.msg = (parent, child) => {
  return (parent || '') + (child || '')
}

app.mount('#app')
// 打印 'Hello Vue'
```
<a name="pqk3u"></a>
#### API

- **createApp() : **创建一个应用实例
   - Vue3和Vue2创建应用实例的方法不同，Vue2使用new Vue()创建应用实例，而Vue3是用createApp()来创建应用实例的
   - 参数：
```javascript
// 第一个参数：根组件；第二个参数可选：要传递给根组件的 props
function createApp(rootComponent: Component, rootProps?: object): App
```

   - 使用
```javascript
import { createApp } from 'vue'
import App from './App.vue'
// 导入需要的组件
const app = createApp(App)
```

- **app.****mount() : **将应用实例挂载在一个容器元素中
   - 参数可以是一个实际的 DOM 元素或一个 CSS 选择器 (使用第一个匹配到的元素)。返回根组件的实例。
   - 对应 API app.unmount()
   - 卸载一个已挂载的应用实例。卸载一个应用会触发该应用组件树内所有组件的卸载生命周期钩子。
```javascript
// 将上面创建的应用实例挂载到id为app的容器中
app.mount('#app')
```

- **app.provide() : **提供一个值，可以在应用中的所有后代组件中注入使用
```javascript
import { createApp } from 'vue'
const app = createApp(/* ... */)
// 提供全局数据
app.provide('message', 'hello')
```

   - inject获取该数据，在应用的某个组件中：
```javascript
import { inject } from 'vue'

export default {
  setup() {
    console.log(inject('message')) // 'hello'
  }
}
```

- **app.component() ：** 注册全局组件或获取已注册组件
   - 如果同时传递一个组件名字符串及其定义，则注册一个全局组件；如果只传递一个名字，则会返回用该名字注册的组件 (如果存在的话)。
```javascript
// 图片上传组件
import ImageUpload from "@/components/ImageUpload"

app.component('ImageUpload', ImageUpload)
```

- **app.directive() :  **创建全局指令或获取已注册的全局指令
   - 如果同时传递一个名字和一个指令定义，则注册一个全局指令；如果只传递一个名字，则会返回用该名字注册的指令 (如果存在的话)。
```javascript
import { createApp } from 'vue'
// 创建应用实例
const app = createApp({
  /* ... */
})
// 注册（对象形式的指令）
app.directive('my-directive', {
  /* 自定义指令钩子 */
})
// 注册（函数形式的指令）
app.directive('my-directive', () => {
  /* ... */
})
// 得到一个已注册的指令
const myDirective = app.directive('my-directive')
```

- **app.use() : 安装一个插件**
```javascript
interface App {
  use(plugin: Plugin, ...options: any[]): this
}
```

   - 第一个参数应是插件本身，可选的第二个参数是要传递给插件的选项。
   - 插件可以是一个带 install() 方法的对象，亦或直接是一个将被用作 install() 方法的函数。插件选项 (app.use() 的第二个参数) 将会传递给插件的 install() 方法。
   - 若 app.use() 对同一个插件多次调用，该插件只会被安装一次。
   - 使用示例：
```javascript
import { createApp } from 'vue'
import MyPlugin from './plugins/MyPlugin'
import router from './router'
import store from './store'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
const app = createApp({
  /* ... */
})

// vue项目中引入路由,vuex等,可连用
app.use(router).use(store)
// 也可引入ui框架
app.use(ElementPlus）
app.use(MyPlugin)
```
<a name="tAIEl"></a>
### 通用API
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703406206339-4cda860b-d1dc-4794-931d-d3d566ab6cb1.webp#averageHue=%23fbfaf9&clientId=u413b99a4-92ec-4&from=paste&id=u70fffaba&originHeight=850&originWidth=1347&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u82d34440-a01b-4f38-b0c1-6cef4b94e67&title=)
<a name="wVt6K"></a>
#### versoin
暴露当前所使用的 Vue 版本。

- **类型** string
- **
```javascript
import { version } from 'vue'

console.log(version)
```
<a name="Qhcdt"></a>
#### nextTick()
等待下一次DOM更新刷新的工具方法

- 类型：
```javascript
function nextTick(callback?: () => void): Promise<void>
```

- 详细信息
   - 当你在 Vue 中更改响应式状态时，最终的 DOM 更新并不是同步生效的，而是由 Vue 将它们缓存在一个队列中，直到下一个“tick”才一起执行。这样是为了确保每个组件无论发生多少状态改变，都仅执行一次更新
   - nextTick() 可以在状态改变后立即使用，以等待 DOM 更新完成。你可以传递一个回调函数作为参数，或者 await 返回的 Promise
```vue
<script setup>
import { ref, nextTick } from 'vue'

const count = ref(0)

async function increment() {
  count.value++

  // DOM 还未更新
  console.log(document.getElementById('counter').textContent) // 0

  await nextTick()
  // DOM 此时已经更新
  console.log(document.getElementById('counter').textContent) // 1
}
</script>

<template>
  <button id="counter" @click="increment">{{ count }}</button>
</template>
```
<a name="YNH8Z"></a>
## 组合式API
谈到**vue3** ，相信大家最为熟悉的就是 **composition API** 了，也就是 **组合式 API** 。
<a name="EFgaz"></a>
### setup
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703420373891-279471a7-1edf-415b-b53a-662bb79ea9ef.webp#averageHue=%23f9f9f9&clientId=u2a9b435f-515f-4&from=paste&id=u55b19a84&originHeight=771&originWidth=1275&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u12c8c81f-168e-4710-b622-d5c7a9762c3&title=)
<a name="tNFve"></a>
### 响应式：核心
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421561873-73c3307a-5a12-4312-94d2-6715ed01674e.webp#averageHue=%23f7f7f7&clientId=u2a9b435f-515f-4&from=paste&id=u95d4b54c&originHeight=1704&originWidth=1561&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u6de85e32-f5ff-4c7d-b4ce-4e50fdb4199&title=)
<a name="tXIij"></a>
### 响应式：工具函数
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421579115-edc69e5d-e3b9-43a1-85fa-6d2cc00279f0.webp#averageHue=%23fafafa&clientId=u2a9b435f-515f-4&from=paste&id=u32bf96c6&originHeight=1003&originWidth=2220&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9c27f243-b121-4ddc-be7b-6f512494173&title=)
<a name="Uir3N"></a>
### 响应式：进阶
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421603349-92d89f15-969e-4035-9ff2-79c137f59409.webp#averageHue=%23fafafa&clientId=u2a9b435f-515f-4&from=paste&id=u22296cbf&originHeight=3507&originWidth=2556&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uda8bbc92-bc93-4025-aec8-7478d36b5db&title=)
<a name="AUyjr"></a>
### 生命周期钩子
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421631149-501bd19c-e6e6-46a3-b81c-883a8e35b0a2.webp#averageHue=%23f7f7f7&clientId=u2a9b435f-515f-4&from=paste&id=uff80b408&originHeight=2028&originWidth=3024&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u49d3c540-3bde-4147-9d2a-d7098829aa0&title=)
<a name="nZQmS"></a>
### 依赖注入
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421644286-1b2048ac-c1d4-4856-b4a6-cb1603bbe5fc.webp#averageHue=%23f7f7f7&clientId=u2a9b435f-515f-4&from=paste&id=u09db3527&originHeight=1045&originWidth=1440&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u8c405d96-9d6d-4182-8d55-1d0ea2f10dc&title=)
<a name="FWF6H"></a>
## 选项式API
**选项式API** 即 **options API** 。主要在VUE2使用
<a name="gQHWM"></a>
### 状态选项
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421704059-63141fc7-da89-4543-b493-d3992a093e61.webp#averageHue=%23fcf9f7&clientId=u2a9b435f-515f-4&from=paste&id=ub8199fd9&originHeight=2062&originWidth=1521&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u43e13364-56ae-42fb-a8b9-7eea11959c0&title=)
<a name="w0tO3"></a>
### 渲染选项
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421741165-c5e14a97-3533-41d2-9f46-6cf3d95c80f0.webp#averageHue=%23faf8f5&clientId=u2a9b435f-515f-4&from=paste&id=uc9095996&originHeight=1059&originWidth=1258&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ue53f4c17-f0e3-4951-bc9a-fa611df3a23&title=)
<a name="SCx8W"></a>
### 生命周期选项
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421762103-6314261f-53b4-4217-aefb-8339b4e6db5a.webp#averageHue=%23fbf9f8&clientId=u2a9b435f-515f-4&from=paste&id=u79f91f3b&originHeight=2382&originWidth=1554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u1764ecd2-4ef1-4140-b79e-fd0bac9e773&title=)
<a name="jh64Y"></a>
### 组合选项
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421774168-70511b81-f92a-47b8-85c6-0bf619f70f24.webp#averageHue=%23fcf6f0&clientId=u2a9b435f-515f-4&from=paste&id=u618470f5&originHeight=693&originWidth=1260&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud2ab82aa-718d-4903-b04d-64ef9566f3b&title=)
<a name="OGsCB"></a>
### 其他杂项
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421793354-3bea0533-cfdf-4c0c-9382-e38f891649eb.webp#averageHue=%23fbf7f2&clientId=u2a9b435f-515f-4&from=paste&id=u31b6b1c7&originHeight=864&originWidth=1282&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf5aaa184-8804-423a-aee7-13b8324c25c&title=)
<a name="FdmKL"></a>
### 组件实例
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421811963-21f7b3a0-c96f-45f2-b3ed-69751698b6e1.webp#averageHue=%23fdf7f4&clientId=u2a9b435f-515f-4&from=paste&id=u75791fa7&originHeight=985&originWidth=1528&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uef4d23e4-738b-4f61-81f9-4c522e3da65&title=)
<a name="tnJFJ"></a>
## 内置内容
vue3 的内置内容包括**指令**、**组件**、**特殊元素element**和**特殊属性attributes**
<a name="f6R6K"></a>
### 指令
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421853788-f650f17c-6e85-49ae-be7a-5b0c4ff1a270.webp#averageHue=%23fdfbf9&clientId=u2a9b435f-515f-4&from=paste&id=ud6a81607&originHeight=2421&originWidth=2701&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u6b1f08e4-4f75-4f82-941a-68038bcb2b6&title=)
<a name="eqhDp"></a>
### 组件
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421863068-ba589f10-eb54-4af8-ae77-5f32e88e5ad2.webp#averageHue=%23fcf9f5&clientId=u2a9b435f-515f-4&from=paste&id=u313b4267&originHeight=2758&originWidth=1726&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u413f04e1-84b2-4a10-8650-4df5dc331cd&title=)
<a name="AKZKQ"></a>
### 特殊元素
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421875141-598267cf-cb06-48eb-8976-2104be2c05c2.webp#averageHue=%23faede8&clientId=u2a9b435f-515f-4&from=paste&id=u927b37ab&originHeight=694&originWidth=1519&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u58b953e0-1901-449e-a481-c98921cf1b4&title=)
<a name="j9jrJ"></a>
### 特殊属性Attributes
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421891753-b42e37b4-b8c6-429f-87d1-8030f4f001e7.webp#averageHue=%23fcf9f5&clientId=u2a9b435f-515f-4&from=paste&id=ue7d0bebd&originHeight=661&originWidth=1627&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc3c8f44c-d28a-4653-99dd-d2f29f07195&title=)
<a name="utMSG"></a>
## 单文件组件
对于 vue 来说，相信大家都会非常熟悉它的组件化思想，似乎有一种理念是：万物皆可组件。那对于一个组件来说，我们都需要了解它的什么内容呢？比如，我们写的 <template> 是什么，用 <script setup> 和 <script lang="ts"> 都分别是什么含义，<style> 用了 scoped 是什么意思，:slotted 插槽选择器又在什么情况下使用呢？我们一起来一探究竟。
<a name="IntLC"></a>
### SFC语法
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421952271-77342936-9a22-4557-b22e-324e41d9f349.webp#averageHue=%23fcfbf9&clientId=u2a9b435f-515f-4&from=paste&id=u1f3d4bd2&originHeight=1926&originWidth=1471&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub3698834-5a45-4830-94e6-838016bb41b&title=)
<a name="s9dlL"></a>
### 单文件组件script setup
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421970221-517ec2b4-7848-4d74-82c5-705d17dd36ef.webp#averageHue=%23fdfdfb&clientId=u2a9b435f-515f-4&from=paste&id=ua78ad3ed&originHeight=1479&originWidth=3024&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ue98946d4-299c-49f2-bc37-f259d9199f4&title=)
<a name="C1aby"></a>
### css功能
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703421987253-e055f006-fa93-4440-af21-6ed1c672cbe4.webp#averageHue=%23fbf5f0&clientId=u2a9b435f-515f-4&from=paste&id=u16a58da8&originHeight=1380&originWidth=2026&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uddfaf9d4-bb75-45f2-9435-ba2a5f7351f&title=)
<a name="MkQU9"></a>
## 进阶API
面我们了解了 vue3 的基础API，准确来说，上面的 API 可以解决实际工作中 80% 的问题。那下面，我们就再来看一些较为进阶的 api 操作。下面所涉及到的这些 API ，更多的是可以在**某些定制化的场景**下，做一些高阶的操作。比如：我们可以在一个 headless 组件里，用 render 和 h() 函数，来渲染自定义的页面。那 进阶 API 都还有哪些东西呢，来看下面的内容。
<a name="dbukP"></a>
### 渲染函数
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703422020634-c1fff523-93c3-4ba0-b1db-0a08f84fa7a1.webp#averageHue=%23fbf8f5&clientId=u2a9b435f-515f-4&from=paste&id=u1331291e&originHeight=1612&originWidth=1852&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uad751240-3287-44eb-8f94-5e8e616e284&title=)
<a name="TNLsf"></a>
### 服务端渲染
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703422035144-63d9b808-86af-4fab-8517-c11ee51126e2.webp#averageHue=%23fcfaf9&clientId=u2a9b435f-515f-4&from=paste&id=ub71c70bc&originHeight=1462&originWidth=2182&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u85d7ef20-a75e-4c68-bfa7-f5799822401&title=)
<a name="WB9Qh"></a>
### TypeScript工具类型
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703422059546-95cc8b74-ac83-4a19-84c3-d593e1f48ffa.webp#averageHue=%23fdfcfc&clientId=u2a9b435f-515f-4&from=paste&id=u70847efc&originHeight=1315&originWidth=1815&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7dcc176f-8dc4-427c-9224-ed99ca2a6dc&title=)
<a name="grs1L"></a>
### 自定义渲染
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1703422072086-6cbeb0bb-b026-4b8e-b8c6-f66d6f9c8160.webp#averageHue=%23faf8f7&clientId=u2a9b435f-515f-4&from=paste&id=uaa3fb377&originHeight=832&originWidth=855&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u979d18f1-7cf6-47f8-981a-8f41628834a&title=)
