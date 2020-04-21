## 几个关键字段的含义

### 1. vuex是什么？

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态。

##### 白话：vuex就是帮我们存储一下多个组件共享的数据，方便我们对其读取和更改。

### 2. State

官方解释：Vuex使用单一状态树，用一个对象就包含了全部的应用层次状态。它便作为一个唯一的数据源而存在。这也就意味着， 每个应用将仅仅包含一个store实例。

##### 白话：组件中所要共享的数据，我们就会抽取一个store，而state即是我们可以共享的数据。

### 3. Mutations

更改Vuex的store中的状态的唯一方法是提交mutation。

Vuex中的mutation非常类似于事件：每个mutation都有一个字符串的事件类型和一个回调函数。

这个回调函数就是我们实际进行状态更改的地方。并且它会接受state第一个参数。

##### 白话：可以理解为更改state的唯一途径就是mutation（同步）

### 4. Actions

类似于Mutation，不同在于：

- Action提交的是mutation，而不是直接更改状态
- Action可以包含任意一步操作

##### 白话：Actions也可以更改state，但是是通过commit，提交到mutation，不直接更改（异步）

### 5. Getters

Vuex 允许我们在 store 中定义`getter`（可以认为是 store 的计算属性）。

就像计算属性 `computed` 一样，`getter` 的返回值会根据它的依赖被缓存起来。

且只有当它的依赖值发生了改变才会被重新计算。

### 6. mapState

是一个辅助函数，当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。

为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性。

##### 白话：即帮我们获取对应的state值

### 7. mapAction

是一个辅助函数，个人觉得比 dispatch 使用起来方便，主要是创建组件方法分发action,推荐使用。

## 关键实操来啦！

### 1. 安装

```
npm install vuex --save
```

### 2. 目录结构 (此项目使用是vue-admin-template脚手架)

用例链接：https://github.com/yzren/Vuex.git

```
- build
- node-modules
- public
- src
    - api
    - components
    - layout
        -  components
           - appMain.vue
           - child.vue
    - router
    - store
        - modules
        - app.js (我们例子中写vuex相关信息文件)
        - index.js
    - app.vue
    - main.js
    
```

### 3. 简单实例

1. 在store文件夹下modules文件夹下添加app.js文件

app.js文件内容：

```
const state = {
  count: 1
}

const mutations = {
  
  SET_ADD_COUNT: (state) => {
    state.count++
  },
  SET_SUBTRACT_COUNT: (state) => {
    state.count--
  }
}

const actions = {
  // 加
  add({ commit }) {
    commit('SET_ADD_COUNT')
  },
  // 减
  subtract({ commit }) {
    commit('SET_SUBTRACT_COUNT')
  }

}

export default {
  // namespaced: true,
  state,
  mutations,
  actions
}
```

2.在store文件夹下新建index.js文件，index内容：

```
import Vue from 'vue'
import Vuex from 'vuex'
import app from './modules/app'

Vue.use(Vuex)

const store = new Vuex.Store({
  modules: {
    app //封装的存放state下count的方法
  }
})

export default store
```

3.在main.js中引入store文件

```
import store from './store'
...
new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App)
})
```

1. 具体到组件中使用，我们以layout内组件为例子

AppMain.vue

```
<template>
  <section class="app-main">
    <h2>父亲组件</h2>
    <p>count计算结果 ：
      <strong>{{ count }}</strong>
    </p>
    <el-button size="small" @click="addFn">+</el-button>
    <el-button size="small" @click="subtractFn">-</el-button>

    <div>
      <child />
    </div>
  </section>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import Child from './chind'

export default {
  name: 'AppMain',
  components: {
    Child
  },
  computed: {
    ...mapState({
      count: state => state.app.count
    })
  },
  methods: {
    ...mapActions(['add', 'subtract']),
    addFn() {
      this.add()
    },
    subtractFn() {
      this.subtract()
    }
  }
}
</script>
...
```

child.vue

```
<template>
  <section class="app-main">
    <h2>子组件</h2>
    <p>count计算结果 ：
      <strong>{{ count }}</strong>
    </p>
    <el-button size="small" @click="addFn">+</el-button>
    <el-button size="small" @click="subtractFn">-</el-button>

  </section>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  name: 'Child',
  computed: {
    ...mapState({
        //引用count
      count: state => state.app.count
    })
  },
  methods: {
    //分发action，即引入之后，就能通过【this.store中的方法名】，
    //调用store内相对应的方法，
    //此例调用的就是action的add和subtract方法
    ...mapActions(['add', 'subtract']),
    
    addFn() {
      this.add()
    },
    subtractFn() {
      this.subtract()
    }
  }
}
</script>
...
```

### 4. 异步实例

主要展示store内文件相关代码，其调用于以上简单实力类似

```
//封装的axios文件
import http from '@common/http';

//封装的需要调用后端的接口文件
import { api } from '@common/config';

//共享的数据
const state = {
    listData: {}
};

const actions = {
    // 获取
    gitTestList({state, commit}, params) {
        // 缓存已请求数据
        if (state.listData[params.id]) {
            return Promise.resolve(state.listData[params.id]);
        }
        return http.get(api.testApi, { params, notLoading: true }).then(res => {
            commit('SET_TEST_LIST', res.data);
            return res.data;
        });
    }
};

const mutations = {
    //设置列表相应信息
    SET_TEST_LIST(state, data) {
        state.listData[data.id] = data;
    }
};

export default {
    state,
    actions,
    mutations
};
```