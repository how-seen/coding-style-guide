# vuex 风格指南

## 目录结构

```
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```

## Getters

Getters 相当于 vue 文件中的 computed 属性。


```javascript
{
  state: {
    user: {
      name: '',
    },
    todos: [
      {
        id: 1
      },
    ],
  },
  getters: {
    username: state => {
      return state.user.name;
    },
    // 通过让 getter 返回一个函数，来实现给 getter 传参
    // 在对 store 里的数组进行查询时非常有用
    geTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id);
    }
  }
}
```

## Mutations

**Mutation 必须是同步函数**

一个 Mutation 函数只能修改 State 中的一个属性，其参数名称应为 State 中的属性名称。

Mutation 的函数命名必须为`全大写形式`，多个单词使用 `_` 分割

风格参考：

```javascript
// mutation-types.js

// 尽量将 mutation 的函数名定义为常量在一个单独的文件中
export const SET_NAME = 'SET_NAME'; 
```

```javascript
// store.js

import { SET_NAME } from './mutation-types'

{
  state: {
    name: ''
  },
  mutations: {
    [SET_NAME] (state, name) {
      state.name = name;
    }
  }
}
```

## Actions

Action 类似于 mutation，不同在于：

Action 提交的是 mutation，而不是直接变更状态。

Action 可以包含任意异步操作。

Action 接收的业务参数应始终命名为 `payload`