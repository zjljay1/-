<a name="YsqDl"></a>
##   VUEX
什么是vuexVuex 是一个专为 Vue.js 应用程序开发的**状态管理模式 + 库**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

<a name="mJjTN"></a>
### 状态管理模式
让我们从一个简单的 Vue 计数应用开始：
```javascript
const Counter = {
  // 状态
  data () {
    return {
      count: 0
    }
  },
  // 视图
  template: `<div>{{ count }}</div>`,
  // 操作
  methods: {
    increment () {
      this.count++
    }
  }
}

createApp(Counter).mount('#app')
```
这个状态自管理应用包含以下几个部分:

- **状态**，驱动应用的数据源；
- **视图**，以声明方式将**状态**映射到视图；
- **操作**，响应在**视图**上的用户输入导致的状态变化。

以下是一个表示“单向数据流”理念的简单示意：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673239815158-9dde764b-ae21-423b-8c2b-aa0aca78c5df.png#averageHue=%23fcf2f2&clientId=ua62f6983-012b-4&from=paste&id=ub41762ff&originHeight=866&originWidth=1280&originalType=url&ratio=1&rotation=0&showTitle=false&size=64230&status=done&style=none&taskId=u0b3f9fd2-9fba-4f93-acd2-5b30e5a5046&title=)但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏：

- 多个视图依赖于同一状态。
- 来自不同视图的行为需要变更同一状态。

对于问题一，传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。对于问题二，我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致无法维护的代码。<br />因此，我们为什么不把组件的共享状态抽取出来，以一个全局单例模式管理呢？在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！<br />通过定义和隔离状态管理中的各种概念并通过强制规则维持视图和状态间的独立性，我们的代码将会变得更结构化且易维护。<br />这就是 Vuex 背后的基本思想，借鉴了 [Flux](https://facebook.github.io/flux/docs/overview)、[Redux](http://redux.js.org/) 和 [The Elm Architecture](https://guide.elm-lang.org/architecture/)。与其他模式不同的是，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673239838740-a869d919-fd5a-47a4-a770-c199054eee35.png#averageHue=%23fefdfb&clientId=ua62f6983-012b-4&from=paste&id=u08f9aa67&originHeight=551&originWidth=701&originalType=url&ratio=1&rotation=0&showTitle=false&size=30485&status=done&style=none&taskId=ucc7054a2-7c7f-4fac-8a1a-8a7180a9f9a&title=)
<a name="iAc84"></a>
### 安装
<a name="DdINf"></a>
#### 直接下载 / CDN引用
[https://unpkg.com/vuex@4](https://unpkg.com/vuex@4)<br />[Unpkg.com](https://unpkg.com/) 提供了基于 npm 的 CDN 链接。以上的链接会一直指向 npm 上发布的最新版本。您也可以通过 https://unpkg.com/vuex@4.0.0/dist/vuex.global.js 这样的方式指定特定的版本。<br />在 Vue 之后引入 vuex 会进行自动安装：
```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```
<a name="xNWHj"></a>
#### npm
```bash
npm install vuex@next --save
```
<a name="H7370"></a>
#### yarn
```bash
yarn add vuex@next --save
```
然后配置 vuex，使其工作起来：在src路径下创建store文件夹，然后创建index.js文件，文件内容如下：
```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    // 定义一个name，以供全局使用
    name: '张三',
    // 定义一个number，以供全局使用
    number: 0,
    // 定义一个list，以供全局使用
    list: [
      { id: 1, name: '111' },
      { id: 2, name: '222' },
      { id: 3, name: '333' },
    ],
  },
});

export default store;
```
```javascript
import Vue from 'vue';
import App from './App';
import router from './router';
import store from './store'; // 引入我们前面导出的store对象

Vue.config.productionTip = false;

new Vue({
  el: '#app',
  router,
  store, // 把store对象添加到vue实例上
  components: { App },
  template: '<App/>',
});
```
```javascript
<template>
  <div></div>
</template>

<script>
export default {
  mounted() {
    // 使用this.$store.state.XXX可以直接访问到仓库中的状态
    console.log(this.$store.state.name);
  },
};
</script>
```
此时，启动项目**npm run dev**，即可在控制台输出刚才我们定义在store中的name的值。![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673240887782-19853e4b-26ed-496b-8298-54c301d5ea38.webp#averageHue=%23f3f3f3&clientId=ua62f6983-012b-4&from=paste&id=u93aeee63&originHeight=130&originWidth=260&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u53480fd8-fe3f-4ff3-a7bf-5b2164e379c&title=)

- 🤖 官方建议1： 官方建议我们以上操作this.$store.state.XXX最好放在计算属性中，当然，我也建议你这么使用，这样可以让你的代码看起来更优雅一些，就像这样：
```javascript
export default {
  mounted() {
    console.log(this.getName);
  },
  computed: {
    getName() {
      return this.$store.state.name;
    },
  },
};
```
此时可以得到和上面一样的效果。

- 🤖 官方建议2： 是不是每次都写this.$store.state.XXX让你感到厌烦，你实在不想写这个东西怎么办，当然有解决方案，就像下面这样：
```html
<script>
  import { mapState } from 'vuex'; // 从vuex中导入mapState
  export default {
    mounted() {
      console.log(this.name);
    },
    computed: {
      ...mapState(['name']), 
      // 经过解构后，自动就添加到了计算属性中，此时就可以直接像访问计算属性一样访问它
    },
  };
</script>
```
你甚至可以在解构的时候给它赋别名，取外号，就像这样：
```javascript
...mapState({ aliasName: 'name' }),  
  // 赋别名的话，这里接收对象，而不是数组
```

<a name="iyQRy"></a>
### Store（**仓库**）
每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

[安装](https://vuex.vuejs.org/zh/installation.html) Vuex 之后，让我们来创建一个 store。创建过程直截了当——仅需要提供一个初始 state 对象和一些 mutation：
```javascript
import { createApp } from 'vue'
import { createStore } from 'vuex'

// 创建一个新的 store 实例
const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

const app = createApp({ /* 根组件 */ })

// 将 store 实例作为插件安装
app.use(store)
```
现在，你可以通过 store.state 来获取状态对象，并通过 store.commit 方法触发状态变更：
```javascript
store.commit('increment')

console.log(store.state.count) // -> 1
```
在 Vue 组件中， 可以通过 this.$store 访问store实例。现在我们可以从组件的方法提交一个变更：
```javascript
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```
再次强调，我们通过提交 mutation 的方式，而非直接改变 store.state.count，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。此外，这样也让我们有机会去实现一些能记录每次状态改变，保存状态快照的调试工具。有了它，我们甚至可以实现如时间穿梭般的调试体验。<br />由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。触发变化也仅仅是在组件的 methods 中提交 mutation。

<a name="vv19R"></a>
### Getter (获取)
:::success
场景试想：

- 🤨 设想一个场景，你已经将store中的name展示到页面上了，而且是很多页面都展示了，此时产品经理过来找事儿😡：
- 产品经理：所有的name前面都要加上“hello”！
- 我：为什么？
- 产品经理：我提需求还需要为什么吗？
- 我：好，我加！
:::
这时候，你第一想到的是怎么加呢，emm...在每个页面上，使用this.$store.state.name获取到值之后，进行遍历，前面追加"hello"即可。<br />🤦🏻‍♂️ 错！这样很不好，原因如下：

- 第一，假如你在A、B、C三个页面都用到了name，那么你要在这A、B、C三个页面都修改一遍，多个页面你就要加很多遍这个方法，造成代码冗余，很不好；
- 第二，假如下次产品经理让你把 “hello” 改成 “fuck” 的时候，你又得把三个页面都改一遍，这时候你只能抽自己的脸了...

👏🏻 吸取上面的教训，你会有一个新的思路：我们可以直接在store中对name进行一些操作或者加工，从源头解决问题！那么具体应该怎么写呢？这时候，本次将要介绍的这个Getter利器闪亮登场！<br />🤡 怎么用呢？不废话，show code！<br />首先，在store对象中增加getters属性
```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    name: '张三',
    number: 0,
    list: [
      { id: 1, name: '111' },
      { id: 2, name: '222' },
      { id: 3, name: '333' },
    ],
  },
  // 在store对象中增加getters属性
  getters: {
    getMessage(state) {
      // 获取修饰后的name，第一个参数state为必要参数，必须写在形参上
      return `hello${state.name}`;
    },
  },
});

export default store;
```
在组件使用
```javascript
export default {
  mounted() {
    // 注意不是$store.state了，而是$store.getters
    console.log(this.$store.state.name);
    console.log(this.$store.getters.getMessage);
  },
};
```
然后查看控制台：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673241304545-296340d9-01ec-4814-a23d-1b2a3a1a89cb.webp#averageHue=%23f4e9e9&clientId=ua62f6983-012b-4&from=paste&id=u9d9a7106&originHeight=160&originWidth=350&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud3fde9fd-5eb8-44e3-a7ea-7377eba6e52&title=)<br />没有问题的<br />🤖 官方建议： 是不是每次都写this.$store.getters.XXX让你感到厌烦，你实在不想写这个东西怎么办，当然有解决方案，官方建议我们可以使用mapGetters去解构到计算属性中，就像使用mapState一样，就可以直接使用this调用了，就像下面这样：
```html
<script>
  import { mapState, mapGetters } from 'vuex';
  export default {
    mounted() {
      console.log(this.name);
      console.log(this.getMessage);
    },
    computed: {
      ...mapState(['name']),
      ...mapGetters(['getMessage']),
    },
  };
</script>
```
此时可以得到和之前一样的效果。<br />当然，和mapState一样你也可以取别名，取外号，就像下面这样：
```javascript
...mapGetters({ aliasName: 'getMessage' }), 
  // 赋别名的话，这里接收对象，而不是数组
```
🤗 OK，当你看到这里，你已经成功的把Getter用起来了，你也能明白在什么时候应该用到getters，你可以通过计算属性访问（缓存），也可以通过方法访问（不缓存），你甚至可以在getters的方法里面再调用getters方法，当然你也实现了像state那样，使用mapGetters解构到计算属性中，这样你就可以很方便的使用getters啦！<br />😎 读取值的操作我们有 “原生读（state）” 和 “修饰读（getters）”，接下来就要介绍怎么修改值了！
<a name="cbSiN"></a>
### Mutation (同步修改)
:::success
场景试想：<br />🤔 假如你打开微信朋友圈，看到你的好友发了动态，但是动态里有个错别字，你要怎么办呢？你可以帮他改掉吗？当然不可以！我们只能通知他本人去修改，因为是别人的朋友圈，你是无权操作的，只有他自己才能操作，同理，在vuex中，我们不能直接修改仓库里的值，必须用vuex自带的方法去修改，这个时候，Mutation闪亮登场了！<br />😬 把问题解释清楚之后，我们准备完成一个效果：我们先输出state中的number的默认值0，然后我们在vue组件里通过提交Mutations改变number的默认值0，改成我们想修改的值，然后再输出出来，这样就可以简单练习怎么使用Mutations了。不说废话，上代码。
:::

- 修改store/index.js
```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    name: '张三',
    number: 0,
  },
  mutations: {
    // 增加nutations属性
    setNumber(state) {
      // 增加一个mutations的方法，方法的作用是让num从0变成5，state是必须默认参数
      state.number = 5;
    },
  },
});

export default store;
```

- 修改App.vue
```html
<script>
export default {
  mounted() {
    console.log(`旧值：${this.$store.state.number}`);
    this.$store.commit('setNumber');
    console.log(`新值：${this.$store.state.number}`);
  },
};
</script>
```

- 运行项目，查看控制台：
- ![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673241754483-a2d5ccb2-9424-4587-adef-2b3b1f6393fe.webp#averageHue=%23f5eae9&clientId=ua62f6983-012b-4&from=paste&id=u81c853f5&originHeight=130&originWidth=400&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u5e4bed89-cca4-4d6e-897f-5f3582ac153&title=)
- 🤡 以上是简单实现mutations的方法，是没有传参的，如果我们想传不固定的参数怎么办？接下来教你解决
- 修改store/index.js
```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    name: '张三',
    number: 0,
  },
  mutations: {
    setNumber(state) {
      state.number = 5;
    },
    setNumberIsWhat(state, number) {
      // 增加一个带参数的mutations方法
      state.number = number;
    },
  },
});

export default store;
```

- 修改App.vue
```html
<script>
  export default {
    mounted() {
      console.log(`旧值：${this.$store.state.number}`);
      this.$store.commit('setNumberIsWhat', 666);
      console.log(`新值：${this.$store.state.number}`);
    },
  };
</script>
```

- 运行项目，查看控制台：
- ![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673241827267-42101456-c008-48dc-b15b-374c2ee8ad30.webp#averageHue=%23f1e6e6&clientId=ua62f6983-012b-4&from=paste&id=u5443dcd5&originHeight=150&originWidth=300&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u3b6e5ab6-4276-47bb-b5c8-d71121f8c90&title=)
- 没有问题！
- 注意：上面的这种传参的方式虽然可以达到目的，但是并不推荐，官方建议传递一个对象进去，这样看起来更美观，对象的名字你可以随意命名，但我们一般命名为payload，代码如下：

修改store/index.js
```javascript
mutations: {
  setNumber(state) {
    state.number = 5;
  },
  setNumberIsWhat(state, payload) {
    // 增加一个带参数的mutations方法，并且官方建议payload为一个对象
    state.number = payload.number;
  },
},
```
```html
<script>
  export default {
    mounted() {
      console.log(`旧值：${this.$store.state.number}`);
      this.$store.commit('setNumberIsWhat', { number: 666 }); // 调用的时候也需要传递一个对象
      console.log(`新值：${this.$store.state.number}`);
    },
  };
</script>
```
**这里说一条重要原则：Mutations里面的函数必须是同步操作，不能包含异步操作**<br />好的，记住这个重要原则，我们再说一个小技巧：

- 🤖 官方建议：就像最开始的mapState和mapGetters一样，我们在组件中可以使用mapMutations以代替this.$store.commit('XXX')，是不是很方便呢？
```html
<script>
import { mapMutations } from 'vuex';
export default {
  mounted() {
    this.setNumberIsWhat({ number: 999 });
  },
  methods: {
    // 注意，mapMutations是解构到methods里面的，而不是计算属性了
    ...mapMutations(['setNumberIsWhat']),
  },
};
</script>
```

- 此时可以得到和之前一样的效果，并且代码又美观了一点！
- 当然你也可以给它叫别名，取外号，就像这样：
```javascript
methods: {
  ...mapMutations({ setNumberIsAlias: 'setNumberIsWhat' }), 
    // 赋别名的话，这里接收对象，而不是数组
},
```
<a name="xN0Iy"></a>
### Action (异步调用Mutation 异步修改)
Actions存在的意义是假设你在修改state的时候有异步操作，vuex作者不希望你将异步操作放在Mutations中，所以就给你设置了一个区域，让你放异步操作，这就是Actions<br />代码：
```javascript
const store = new Vuex.Store({
  state: {
    name: '张三',
    number: 0,
  },
  mutations: {
    setNumberIsWhat(state, payload) {
      state.number = payload.number;
    },
  },
  actions: {
    // 增加actions属性
    setNum(content) {
      // 增加setNum方法，默认第一个参数是content，其值是复制的一份store
      return new Promise(resolve => {
        // 我们模拟一个异步操作，1秒后修改number为888
        setTimeout(() => {
          content.commit('setNumberIsWhat', { number: 888 });
          resolve();
        }, 1000);
      });
    },
  },
});
```
```javascript
async mounted() {
  console.log(`旧值：${this.$store.state.number}`);
  await this.$store.dispatch('setNum');
  console.log(`新值：${this.$store.state.number}`);
},
```

- 运行项目，查看控制台：
- ![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673242207024-febe7dd2-28dd-4426-8158-a73ecb68a951.webp#averageHue=%23f4f0f0&clientId=ua62f6983-012b-4&from=paste&id=u75ae17cf&originHeight=200&originWidth=370&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uaa3e8d38-d1db-4da6-85d8-e9156df5c4b&title=)
- 🤓 action就是去提交mutation的，什么异步操作都在action中消化了，最后再去提交mutation的。

😼 当然，你可以模仿mutation进行传参，就像下面这样：
```javascript
actions: {
  setNum(content, payload) {
    // 增加payload参数
    return new Promise(resolve => {
      setTimeout(() => {
        content.commit('setNumberIsWhat', { number: payload.number });
        resolve();
      }, 1000);
    });
  },
},
```
```javascript
async mounted() {
  console.log(`旧值：${this.$store.state.number}`);
  await this.$store.dispatch('setNum', { number: 611 });
  console.log(`新值：${this.$store.state.number}`);
},
```
没有任何问题！

- 🤖 官方建议1：你如果不想一直使用this.$store.dispatch('XXX')这样的写法调用action，你可以采用mapActions的方式，把相关的actions解构到methods中，用this直接调用：
```html
<script>
  import { mapActions } from 'vuex';
  export default {
    methods: {
      ...mapActions(['setNum']), // 就像这样，解构到methods中
    },
    async mounted() {
      await this.setNum({ number: 123 }); // 直接这样调用即可
    },
  };
</script>
```
当然，你也可以取别名，取外号，就像下面这样：
```javascript
...mapActions({ setNumAlias: 'setNum' }),  
  // 赋别名的话，这里接收对象，而不是数组
```
🤖 官方建议2：在store/index.js中的actions里面，方法的形参可以直接将commit解构出来，这样可以方便后续操作
```javascript
actions: {
  setNum({ commit }) {
    // 直接将content结构掉，解构出commit，下面就可以直接调用了
    return new Promise(resolve => {
      setTimeout(() => {
        commit('XXXX'); // 直接调用
        resolve();
      }, 1000);
    });
  },
},
```
🤠 OK，看到这里，你应该明白action在vuex的位置了吧，什么时候该用action，什么时候不用它，你肯定有了自己的判断，最主要的判断条件就是我要做的操作是不是异步，这也是action存在的本质。当然，你不要将action和mutation混为一谈，action其实就是mutation的上一级，在action这里处理完异步的一些操作后，后面的修改state就交给mutation去做了。
<a name="mQeI0"></a>
### Module （模块）
<a name="n18Uk"></a>
#### 按属性进行拆分
🤯 接下来我们想象一下，目前我们介绍的store/index.js里面的内容是非常少的，如果你是一个稍微有些规格的项目，那么你将会得到一个成百上千行的index.js，然后查找一些东西就会非常费劲，我们建议你的一个文件内的行数尽量不要超过200行，不然对于调试来说没有好处。既然问题出来了，我们看一下怎么拆分一下。<br />🤒 我们看到，一个store/index.js里面大致包含state/getters/mutations/actions这四个属性，我们可以彻底点，index.js里面就保持这个架子，把里面的内容四散到其他文件中。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673242890591-4fb9eecf-b716-4f68-828a-794f234c089b.webp#averageHue=%2324251f&clientId=ua62f6983-012b-4&from=paste&id=uc5810deb&originHeight=638&originWidth=549&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u05719f24-4cc8-4601-bb46-cf154020eae&title=)<br />于是我们可以这样拆分：<br />新建四个文件，分别是**state.js** **getters.js** **mutations.js** **actions.js**：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673242915122-07ddeb41-8a48-434e-9185-4e23a659eb5b.webp#averageHue=%233e3c2e&clientId=ua62f6983-012b-4&from=paste&id=u6c587b78&originHeight=193&originWidth=318&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u85c3d105-f5a9-48c6-a848-0d09ab4bfc0&title=)<br />拆出来**state**放到**state.js**中：
```javascript
// state.js
export const state = {
  name: '张三',
  number: 0,
  list: [
    { id: 1, name: '111' },
    { id: 2, name: '222' },
    { id: 3, name: '333' },
  ],
};
```
拆出来**getters**放到**getters.js**中：
```javascript
// getters.js

export const getters = {
  getMessage(state) {
    return `hello${state.name}`;
  },
};
```
出来**mutations**放到**mutations.js**中：
```javascript
// mutations.js

export const mutations = {
  setNumber(state) {
    state.number = 5;
  },
};
```
拆出来**actions**放到**actions.js**中：
```javascript
// actions.js

export const actions = {
  setNum(content) {
    return new Promise(resolve => {
      setTimeout(() => {
        content.commit('setNumberIsWhat', { number: 888 });
        resolve();
      }, 1000);
    });
  },
};
```
组装到主文件里面：
```javascript
import Vue from 'vue';
import Vuex from 'vuex';
import { state } from './state'; // 引入 state
import { getters } from './getters'; // 引入 getters
import { mutations } from './mutations'; // 引入 mutations
import { actions } from './actions'; // 引入 actions

Vue.use(Vuex);

const store = new Vuex.Store({
  state: state,
  getters: getters,
  mutations: mutations,
  actions: actions,
});

// 可以简写成下面这样：

// const store = new Vuex.Store({ state, getters, mutations, actions });

export default store;
```
🤓 以上就是简单的进行了按属性进行拆分store里面的代码，这样就比较清晰了哈，你需要加什么就去哪里加，大家各干各的，互不影响。<br />当然，你完全可以不这么做，引用官方文档中的一句话，“**需要多人协作的大型项目中，这会很有帮助。但如果你不喜欢，你完全可以不这样做**”。
<a name="UBmMC"></a>
#### 按功能进行拆分 - Module
😜 上面我们介绍如何拆分项目，采用的是按**属性**的方式去拆分，将getters/actions/mutations等属性拆分到不同的文件中。<br />接下来，我们介绍一下按另外的一个维度去拆分我们的store，‘按**功能**’，按功能拆分的话，就是我们的标题 **Module(模块)** 。<br />🤖 我们先来看下官方文档是怎么介绍Module的：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1673243104439-f6fe7c08-eddc-44f5-a631-40eacf3a315f.webp#averageHue=%23bcc4bf&clientId=ua62f6983-012b-4&from=paste&id=u463fc687&originHeight=984&originWidth=1065&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uce8245ac-a9f7-4b6a-8ca5-59781af0fe9&title=)

- 看了图中的描述，你或许已经区分了这里使用的**按功能拆分Module**和我们上次介绍的**按属性拆分**的异同了；就像图中的场景一样，我们有一个总store，在这里面根据不同的功能，我们加了两个不同的store，每个store里面维护自己的state，以及自己的actions/mutations/getters。
- 🤡 不说废话，我们用代码实现一下
1. 我们在之前的store上，增加一个新的仓库store2，主要代码如下：
```javascript
// store2.js

const store2 = {
  state: {
    name: '我是store2',
  },
  mutations: {},
  getters: {},
  actions: {},
};

export default store2;
```
然后在store中引入我们新创建的store2模块
```javascript
import Vue from 'vue';
import Vuex from 'vuex';
import { state } from './state';
import { getters } from './getters';
import { mutations } from './mutations';
import { actions } from './actions';
import store2 from './store2'; // 引入store2模块

Vue.use(Vuex);

const store = new Vuex.Store({
  modules: { store2 }, // 把store2模块挂载到store里面
  state: state,
  getters: getters,
  mutations: mutations,
  actions: actions,
});

export default store;
```
访问state - 我们在App.vue测试访问store2模块中的state中的name，结果如下：
```html
<template>
  <div></div>
</template>

<script>
  export default {
    mounted() {
      console.log(this.$store.state.store2.name); // 访问store2里面的name属性
    },
  };
</script>
```
我们通过下面的代码可以了解到在不同的属性里是怎么访问 **模块内的状态** 或者 **根状态**
```javascript
mutations: {
  changeName(state, payload) {
    // state 局部状态
    console.log(state);
    console.log(payload.where);
  },
},
getters: {
  testGetters(state, getters, rootState) {
    // state 局部状态
    console.log(state);
    // 局部 getters,
    console.log(getters);
    // rootState 根节点状态
    console.log(rootState);
  },
},
actions: {
  increment({ state, commit, rootState }) {
    // state 局部状态
    console.log(state);
    // rootState 根节点状态
    console.log(rootState);
  },
},
```
以上是对module的简单介绍，其实这里就是一种思想，分而治之，将复杂的进行拆分，可以更有效的管理。<br />其实以上并不是module的全部，还有一些比如命名空间、模块注册全局 action、带命名空间的绑定函数、模块动态注册、模块重用等方法这里就没介绍，如果你在项目中使用到了，再进行查阅即可，有时候不需要完全理解，知道有这个东西就行，知道出了问题的时候该去哪查资料就够啦。😊
<a name="socrF"></a>
## Pinia
pinia官网：<br />[Pinia 🍍](https://pinia.vuejs.org/zh/introduction.html)<br />参考链接<br />[新一代状态管理工具，Pinia.js 上手指南 - 掘金](https://juejin.cn/post/7049196967770980389#heading-11)
<a name="ZCMSZ"></a>
### 创建Store
新建 src/store 目录并在其下面创建 index.ts，导出 store
```typescript
// src/store/index.ts

import { createPinia } from 'pinia'

const store = createPinia()

export default store
```
在 main.ts 中引入并使用。
```typescript
// src/main.ts

import { createApp } from 'vue'
import App from './App.vue'
import store from './store'

const app = createApp(App)
app.use(store)
```
<a name="tWo5t"></a>
### Store
在 src/store 下面创建一个user.ts
```typescript
//src/store/user.ts

import { defineStore } from 'pinia'

export const useUserStore = defineStore({
  id: 'user', // id必填，且需要唯一
  state: () => {
    return {
      name: '张三'
    }
  }
})
```
<a name="miuF3"></a>
#### 获取State
```typescript
<template>
  <div>{{ userStore.name }}</div>
</template>

<script lang="ts" setup>
import { useUserStore } from '@/store/user'

const userStore = useUserStore()
</script>
```
也可以结合 computed 获取。
```typescript
const name = computed(() => userStore.name)
```
state 也可以使用解构，但使用解构会使其失去响应式，这时候可以用 pinia 的 **storeToRefs**。
```typescript
import { storeToRefs } from 'pinia'
const { name } = storeToRefs(userStore)
```
<a name="kFZ0o"></a>
#### 修改state
可以像下面这样直接修改 state
```typescript
userStore.name = '李四'
```
但一般不建议这么做，建议通过 actions 去修改 state，action 里可以直接通过 this 访问。
```typescript
export const useUserStore = defineStore({
  id: 'user',
  state: () => {
    return {
      name: '张三'
    }
  },
  actions: {
    updateName(name) {
      this.name = name
    }
  }
})
```
```typescript
<script lang="ts" setup>
import { useUserStore } from '@/store/user'

const userStore = useUserStore()
userStore.updateName('李四')
</script>
```
<a name="QXpXI"></a>
### Getters
```typescript
export const useUserStore = defineStore({
 id: 'user',
 state: () => {
   return {
     name: '张三'
   }
 },
 getters: {
   fullName: (state) => {
     return state.name + '丰'
   }
 }
})
```
```typescript
userStore.fullName // 张三丰
```
<a name="FR4io"></a>
### Actions
<a name="kqbDX"></a>
#### 异步actions
action 可以像写一个简单的函数一样支持 async/await 的语法，让你愉快的应付异步处理的场景。
```typescript
export const useUserStore = defineStore({
  id: 'user',
  actions: {
    async login(account, pwd) {
      const { data } = await api.login(account, pwd)
      return data
    }
  }
})
```
<a name="qLKSv"></a>
#### actions 间相互调用
action 间的相互调用，直接用 this 访问即可。
```typescript
 export const useUserStore = defineStore({
  id: 'user',
  actions: {
    async login(account, pwd) {
      const { data } = await api.login(account, pwd)
      this.setData(data) // 调用另一个 action 的方法
      return data
    },
    setData(data) {
      console.log(data)
    }
  }
})
```
在 action 里调用其他 store 里的 action 也比较简单，引入对应的 store 后即可访问其内部的方法了。
```typescript
// src/store/user.ts

import { useAppStore } from './app'
export const useUserStore = defineStore({
  id: 'user',
  actions: {
    async login(account, pwd) {
      const { data } = await api.login(account, pwd)
      const appStore = useAppStore()
      appStore.setData(data) // 调用 app store 里的 action 方法
      return data
    }
  }
})
```
```typescript
// src/store/app.ts

export const useAppStore = defineStore({
  id: 'app',
  actions: {
    setData(data) {
      console.log(data)
    }
  }
})
```

<a name="PkmHm"></a>
### 数据持久化
插件 pinia-plugin-persist 可以辅助实现数据持久化功能。
<a name="LroAY"></a>
#### <br /> 安装
```typescript
npm i pinia-plugin-persist --save
```
<a name="st0SB"></a>
#### 使用
```typescript
// src/store/index.ts

import { createPinia } from 'pinia'
import piniaPluginPersist from 'pinia-plugin-persist'

const store = createPinia()
store.use(piniaPluginPersist)

export default store
```
接着在对应的 store 里开启 persist 即可。
```typescript
export const useUserStore = defineStore({
  id: 'user',

  state: () => {
    return {
      name: '张三'
    }
  },

  // 开启数据缓存
  persist: {
    enabled: true
  }
})
```
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1676989099375-2c085aaf-0778-4356-9517-571eb21bdd23.webp#averageHue=%23f1f1ec&clientId=u80d7b066-449c-4&from=paste&id=u78c1e1dc&originHeight=440&originWidth=1920&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u12c6b74a-2eee-4a66-82ec-b756f4da38d&title=)<br />数据默认存在 sessionStorage 里，并且会以 store 的 id 作为 key。
<a name="qg1bZ"></a>
#### <br /> 自定义key
你也可以在 strategies 里自定义 key 值，并将存放位置由 sessionStorage 改为 localStorage。
```typescript
persist: {
  enabled: true,
    strategies: [
    {
      key: 'my_user',
      storage: localStorage,
    }
  ]
}
```
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1676989132711-59dec04a-5919-4528-9887-2223bca68ba9.webp#averageHue=%23f4f1e7&clientId=u80d7b066-449c-4&from=paste&id=ucba2d9ba&originHeight=432&originWidth=1920&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u1c710822-de0e-41e8-8631-bb868cb5658&title=)
<a name="ADcjm"></a>
#### 持久化部分state
默认所有 state 都会进行缓存，你可以通过 paths 指定要持久化的字段，其他的则不会进行持久化。
```typescript
state: () => {
  return {
    name: '张三',
    age: 18,
    gender: '男'
  }  
},

  persist: {
  enabled: true,
    strategies: [
    {
      storage: localStorage,
      paths: ['name', 'age']
    }
  ]
}
```
上面我们只持久化 name 和 age，并将其改为localStorage, 而 gender 不会被持久化，如果其状态发生更改，页面刷新时将会丢失，重新回到初始状态，而 name 和 age 则不会。

 
