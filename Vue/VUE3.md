<a name="lqhAn"></a>
## 插槽
用于允许父组件向子组件传递内容。它允许你在子组件中定义一些预留位置，然后在父组件中填充具体的内容，使得父组件可以动态地向子组件传递内容
<a name="v5CeZ"></a>
### 插槽内容和出口
![image.png](https://cdn.nlark.com/yuque/0/2024/png/28163149/1709277485625-2968e5fa-adff-4c31-9549-cf7c74370c66.png#averageHue=%23060303&clientId=u081d2271-9454-4&from=paste&id=uceec69e9&originHeight=520&originWidth=1376&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=46797&status=done&style=none&taskId=u1b3ba98c-887a-4ce0-8451-a93533763b2&title=)
```html
<!-- 父组件 -->
<FancyButton>
  Click me <!-- 插槽内容 -->
</FancyButton>

// 子组件
<button class="fancy-btn">
  <slot></slot> <!-- 插槽出口 -->
</button>
```
<slot> 元素是一个**插槽出口** (slot outlet)，标示了父元素提供的**插槽内容** (slot content) 将在哪里被渲染。<br />最终渲染出的 DOM 是这样：
```html
<button class="fancy-btn">Click me!</button>
```
<a name="xDEaT"></a>
### 默认内容
在外部没有提供任何内容的情况下，可以为插槽指定默认内容。比如有这样一个 <SubmitButton> 组件：
```html
<button type="submit">
  <slot></slot>
</button>
```
如果我们想在父组件没有提供任何插槽内容时在 <button> 内渲染“Submit”，只需要将“Submit”写在 <slot> 标签之间来作为默认内容：
```html
<button type="submit">
  <slot>
    Submit <!-- 默认内容 -->
  </slot>
</button>
```
现在，当我们在父组件中使用 <SubmitButton> 且没有提供任何插槽内容时：
```html
<SubmitButton />
```
“Submit”将会被作为默认内容渲染：
```html
<button type="submit">Submit</button>
```
但如果我们提供了插槽内容： 
```html
<SubmitButton>Save</SubmitButton>
```
那么被显式提供的内容会取代默认内容：
```html
<button type="submit">Save</button>
```
<a name="vIrXl"></a>
### 具名插槽
有时在一个组件中包含多个插槽出口是很有用的。举例来说，在一个 <BaseLayout> 组件中，有如下模板：
```html
<div class="container">
  <header>
    <!-- 标题内容放这里 -->
  </header>
  <main>
    <!-- 主要内容放这里 -->
  </main>
  <footer>
    <!-- 底部内容放这里 -->
  </footer>
</div>
```
对于这种场景，<slot> 元素可以有一个特殊的 attribute name，用来给各个插槽分配唯一的 ID，以确定每一处要渲染的内容：
```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
<br />这类带 name 的插槽被称为具名插槽 (named slots)。没有提供 name 的 <slot> 出口会隐式地命名为“default”。<br />在父组件中使用 <BaseLayout> 时，我们需要一种方式将多个插槽内容传入到各自目标插槽的出口。此时就需要用到**具名插槽**了：<br />要为具名插槽传入内容，我们需要使用一个含 v-slot 指令的 <template> 元素，并将目标插槽的名字传给该指令：
```html
<BaseLayout>
  <template v-slot:header>
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```
v-slot 有对应的简写 #，因此 <template v-slot:header> 可以简写为 <template #header>。其意思就是“将这部分模板片段传入子组件的 header 插槽中”<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/28163149/1709278230427-eda67fe1-1535-46f5-b69e-06d757a56b66.png#averageHue=%23060202&clientId=u081d2271-9454-4&from=paste&id=u42c53a38&originHeight=640&originWidth=1376&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=70381&status=done&style=none&taskId=u30b4a788-6112-4a2a-a955-0c2e0e75cfa&title=)<br />下面我们给出完整的、向 <BaseLayout> 传递插槽内容的代码，指令均使用的是缩写形式：
```html
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```
当一个组件同时接收默认插槽和具名插槽时，所有位于顶级的非 <template> 节点都被隐式地视为默认插槽的内容。所以上面也可以写成：
```html
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <!-- 隐式的默认插槽 -->
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```
现在 <template> 元素中的所有内容都将被传递到相应的插槽。最终渲染出的 HTML 如下：
```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```
<a name="j5Bg7"></a>
### 动态插槽名
动态指令参数在 v-slot 上也是有效的，即可以定义下面这样的动态插槽名：
```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>

  <!-- 缩写为 -->
  <template #[dynamicSlotName]>
    ...
  </template>
</base-layout>
```
<a name="HBTbz"></a>
### 作用域插槽
插槽的内容可能想要同时使用父组件域内和子组件域内的数据。要做到这一点，我们需要一种方法来让子组件在渲染时将一部分数据提供给插槽。<br />我们也确实有办法这么做！可以像对组件传递 props 那样，向一个插槽的出口上传递 attributes：
```html
<!-- <MyComponent> 的模板 -->
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>
```
当需要接收插槽 props 时，默认插槽和具名插槽的使用方式有一些小区别。下面我们将先展示默认插槽如何接受 props，通过子组件标签上的 v-slot 指令，直接接收到了一个插槽 props 对象：
```html
<MyComponent v-slot="slotProps">
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>
```
![](https://cdn.nlark.com/yuque/0/2024/svg/28163149/1709278976425-8c3c54e6-0e6e-4337-9699-2c748af3847e.svg#clientId=u081d2271-9454-4&from=paste&id=u1287cca9&originHeight=425&originWidth=678&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u8cb1ba48-1353-4f6a-80f5-f323d256662&title=)

具名作用域插槽的工作方式也是类似的，插槽 props 可以作为 v-slot 指令的值被访问到：v-slot:name="slotProps"。当使用缩写时是这样：
```html
<MyComponent>
  <template #header="headerProps">
    {{ headerProps }}
  </template>

  <template #default="defaultProps">
    {{ defaultProps }}
  </template>

  <template #footer="footerProps">
    {{ footerProps }}
  </template>
</MyComponent>
```
向具名插槽中传入 props：
```html
<slot name="header" message="hello"></slot>
```
注意插槽上的 name 是一个 Vue 特别保留的 attribute，不会作为 props 传递给插槽。因此最终 headerProps 的结果是 { message: 'hello' }。<br />如果你同时使用了具名插槽与默认插槽，则需要为默认插槽使用显式的 <template> 标签。尝试直接为组件添加 v-slot 指令将导致编译错误。这是为了避免因默认插槽的 props 的作用域而困惑。举例：
```html
<!-- 该模板无法编译 -->
<template>
  <MyComponent v-slot="{ message }">
    <p>{{ message }}</p>
    <template #footer>
      <!-- message 属于默认插槽，此处不可用 -->
      <p>{{ message }}</p>
    </template>
  </MyComponent>
</template>
```
为默认插槽使用显式的 <template> 标签有助于更清晰地指出 message 属性在其他插槽中不可用：
```html
<template>
  <MyComponent>
    <!-- 使用显式的默认插槽 -->
    <template #default="{ message }">
      <p>{{ message }}</p>
    </template>

    <template #footer>
      <p>Here's some contact info</p>
    </template>
  </MyComponent>
</template>
```
<a name="tUVTC"></a>
## 依赖注入
<a name="ByX59"></a>
### Prop逐级透传问题
通常情况下，当我们需要从父组件向子组件传递数据时，会使用 [props](https://cn.vuejs.org/guide/components/props.html)。想象一下这样的结构：有一些多层级嵌套的组件，形成了一颗巨大的组件树，而某个深层的子组件需要一个较远的祖先组件中的部分数据。在这种情况下，如果仅使用 props 则必须将其沿着组件链逐级传递下去，这会非常麻烦：<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/28163149/1709280898266-95aa42f5-4d33-4634-ad55-5a33a5d118f8.png#averageHue=%23020101&clientId=u081d2271-9454-4&from=paste&id=ypBwa&originHeight=584&originWidth=1376&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=33089&status=done&style=none&taskId=u1b5a7006-27fc-42af-87d6-dbc1b6908f6&title=)<br />注意，虽然这里的 <Footer> 组件可能根本不关心这些 props，但为了使 <DeepChild> 能访问到它们，仍然需要定义并向下传递。如果组件链路非常长，可能会影响到更多这条路上的组件。这一问题被称为“prop 逐级透传”，显然是我们希望尽量避免的情况。<br />provide 和 inject 可以帮助我们解决这一问题。 [[1]](https://cn.vuejs.org/guide/components/provide-inject.html#footnote-1) 一个父组件相对于其所有的后代组件，会作为**依赖提供者**。任何后代的组件树，无论层级有多深，都可以**注入**由父组件提供给整条链路的依赖。<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/28163149/1709280919119-65f96d4f-7793-4ccd-8559-c530e608b278.png#averageHue=%23010000&clientId=u081d2271-9454-4&from=paste&id=u7138871b&originHeight=584&originWidth=1376&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=36371&status=done&style=none&taskId=u7b2bacb8-ca33-457d-a3b8-a5ed0dcc3f8&title=)
<a name="jkLpv"></a>
### Provide(提供)
要为组件后代提供数据，需要使用到 [provide()](https://cn.vuejs.org/api/composition-api-dependency-injection.html#provide) 函数：
```html
<script setup>
import { provide } from 'vue'

provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
</script>
```
如果不使用 <script setup>，请确保 provide() 是在 setup() 同步调用的：
```javascript
import { provide } from 'vue'

export default {
  setup() {
    provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
  }
}
```
provide() 函数接收两个参数。第一个参数被称为**注入名**，可以是一个字符串或是一个 Symbol。后代组件会用注入名来查找期望注入的值。一个组件可以多次调用 provide()，使用不同的注入名，注入不同的依赖值。

第二个参数是提供的值，值可以是任意类型，包括响应式的状态，比如一个 ref：
```javascript
import { ref, provide } from 'vue'

const count = ref(0)
provide('key', count)
```
提供的响应式状态使后代组件可以由此和提供者建立响应式的联系。
<a name="RWG2r"></a>
### 应用层Provide
除了在一个组件中提供依赖，我们还可以在整个应用层面提供依赖：
```javascript
import { createApp } from 'vue'

const app = createApp({})

app.provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
```
在应用级别提供的数据在该应用内的所有组件中都可以注入。这在你编写插件时会特别有用，因为插件一般都不会使用组件形式来提供值。<br /> 
<a name="CZC3S"></a>
### Inject（注入）
要注入上层组件提供的数据，需使用 [inject()](https://cn.vuejs.org/api/composition-api-dependency-injection.html#inject) 函数：
```vue
<script setup>
  import { inject } from 'vue'

  const message = inject('message')
</script>
```
如果提供的值是一个 ref，注入进来的会是该 ref 对象，而**不会**自动解包为其内部的值。这使得注入方组件能够通过 ref 对象保持了和供给方的响应性链接。<br />同样的，如果没有使用 <script setup>，inject() 需要在 setup() 内同步调用：
```javascript
import { inject } from 'vue'

export default {
  setup() {
    const message = inject('message')
    return { message }
  }
}
```
<a name="Bzqph"></a>
### 注入默认值
默认情况下，inject 假设传入的注入名会被某个祖先链上的组件提供。如果该注入名的确没有任何组件提供，则会抛出一个运行时警告。<br />如果在注入一个值时不要求必须有提供者，那么我们应该声明一个默认值，和 props 类似：
```javascript
// 如果没有祖先组件提供 "message"
// `value` 会是 "这是默认值"
const value = inject('message', '这是默认值')
```
在一些场景中，默认值可能需要通过调用一个函数或初始化一个类来取得。为了避免在用不到默认值的情况下进行不必要的计算或产生副作用，我们可以使用工厂函数来创建默认值：<br />第三个参数表示默认值应该被当作一个工厂函数。
```javascript
const value = inject('key', () => new ExpensiveClass(), true)
```
<a name="fvqvh"></a>
### 和响应式数据配合使用
当提供 / 注入响应式的数据时，**建议尽可能将任何对响应式状态的变更都保持在供给方组件中**。这样可以确保所提供状态的声明和变更操作都内聚在同一个组件内，使其更容易维护。<br />有的时候，我们可能需要在注入方组件中更改数据。在这种情况下，我们推荐在供给方组件内声明并提供一个更改数据的方法函数：

```vue
<!-- 在供给方组件内 -->
<script setup>
  import { provide, ref } from 'vue'

  const location = ref('North Pole')

  function updateLocation() {
    location.value = 'South Pole'
  }

  provide('location', {
    location,
    updateLocation
  })
</script>
```
```vue
<!-- 在注入方组件 -->
<script setup>
  import { inject } from 'vue'

  const { location, updateLocation } = inject('location')
</script>

<template>
  <button @click="updateLocation">{{ location }}</button>
</template>
```
最后，如果你想确保提供的数据不能被注入方的组件更改，你可以使用 [readonly()](https://cn.vuejs.org/api/reactivity-core.html#readonly) 来包装提供的值。
```vue
<script setup>
import { ref, provide, readonly } from 'vue'

const count = ref(0)
provide('read-only-count', readonly(count))
</script>
```
<a name="l37i6"></a>
### 使用 Symbol 作注入名
至此，我们已经了解了如何使用字符串作为注入名。但如果你正在构建大型的应用，包含非常多的依赖提供，或者你正在编写提供给其他开发者使用的组件库，建议最好使用 Symbol 来作为注入名以避免潜在的冲突。<br />我们通常推荐在一个单独的文件中导出这些注入名 Symbol：
```javascript
// keys.js
export const myInjectionKey = Symbol()
```
```javascript
// 在供给方组件中
import { provide } from 'vue'
import { myInjectionKey } from './keys.js'

provide(myInjectionKey, { /*
  要提供的数据
*/ });
```
```javascript
// 注入方组件
import { inject } from 'vue'
import { myInjectionKey } from './keys.js'

const injected = inject(myInjectionKey)
```
<a name="uwxxv"></a>
## history模式与hash模式
<a name="WDc9B"></a>
## 自定义hook
<a name="TAoQB"></a>
## toRef与toRefs
<a name="F5h3t"></a>
## 常用函数
<a name="st27E"></a>
### setup
setup 函数也是 Composition API 的入口函数，我们的变量、方法都是在该函数里定义的，来看一下使用方法
```vue
<template>
  <div id="app">
    <p>{{ number }}</p>
    <button @click="add">增加</button>
  </div>
</template>

<script>
  // 1. 从 vue 中引入 ref 函数
  import {ref} from 'vue'
  export default {
    name: 'App',
    setup() {
      // 2. 用 ref 函数包装一个响应式变量 number
      let number = ref(0)

      // 3. 设定一个方法
      function add() {
        // number是被ref函数包装过了的，其值保存在.value中
        number.value ++
      }

      // 4. 将 number 和 add 返回出去，供template中使用
      return {number, add}
    }

  }
</script>

```
