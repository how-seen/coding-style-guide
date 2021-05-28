# vue 文件风格指南

## 完整单词的组件名

组件名应该倾向于完整单词而不是缩写。

```html
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```

## 为组件样式设置作用域

对于应用来说，顶级 App 组件和布局组件中的样式可以是全局的，但是其它所有组件都应该是有作用域的。

```html
<template>
  <button class="button button-close">X</button>
</template>

<!-- 使用 `scoped` attribute -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>
```

## 单文件组件文件名称

单文件组件的文件名始终是单词大写开头 (PascalCase)

```
MyComponent.vue
```

## 组件名为多个单词

组件名应该始终是多个单词的, 这样做可以避免跟现有的以及未来的 HTML 元素相冲突，因为所有的 HTML 元素名称都是单个单词的。

```javascript
Vue.component('todo-item', {
  // ...
});

export default {
  name: 'TodoItem',
  // ...
}
```

## 紧密耦合的组件名

和父组件紧密耦合的子组件应该以父组件名作为前缀命名。

```
components/
|─ TodoList.vue
|─ TodoListItem.vue
└─ TodoListItemButton.vue
```

## 自闭合组件

在单文件组件中没有内容的组件应该是自闭合的。

```
<my-component />
```

## 为 v-for 设置键值

总是用 key 配合 v-for。

```html
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

## 避免 v-if 和 v-for 用在一起

永远不要把 v-if 和 v-for 同时用在同一个元素上。

```html
<!--
  为了过滤一个列表中的项目 (比如 v-for="user in users" v-if="user.isActive")。在这种情形下，请将 users 替换为一个计算属性 (比如 activeUsers)，让其返回过滤后的列表。
 -->
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

<!-- 
  为了避免渲染本应该被隐藏的列表 (比如 v-for="user in users" v-if="shouldShowUsers")。这种情形下，请将 v-if 移动至容器元素上 (比如 ul、ol)。
 -->
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

## Prop 定义

Prop 定义应该尽量详细, 至少需要指定其类型。

```javascript
// 至少指定其类型
props: {
  status: String
}

// 更好的做法！
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
  }
}
```

## Prop 名大小写

在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板中应该始终使用 kebab-case。

```javascript
export default {
  props: {
    greetingText: String,
  },
};
```

```html
<welcome-message greeting-text="hi" />
```

## scoped 中的元素选择器

元素选择器应该避免在 scoped 中出现。

在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

```html
<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```

## 指令缩写

指令缩写，用 : 表示 v-bind: ，用 @ 表示 v-on: ，用 # 表示 v-slot:

```html
<input :value="value" @input="handleInput" />

<template #header>
  <h1>Here might be a page title</h1>
</template>
```

## Props 顺序

标签的 Props 应该有统一的顺序，依次为指令、属性和事件。

```html
<my-component
  v-if="if"
  v-show="show"
  v-model="value"
  ref="ref"
  :key="key"
  :text="text"
  @input="handleInput"
  @change="handleChange"
/>
```

## 组件选项的顺序

组件选项应该有统一的顺序。

```javascript
export default {
  name: '',

  mixins: [],

  components: {},

  props: {},

  data() {},

  computed: {},

  watch: {},

  created() {},

  mounted() {},

  destroyed() {},

  methods: {},
};
```

## 组件选项中的空行

组件选项较多时，建议在属性之间添加空行。

```javascript
export default {
  computed: {
    formattedValue() {
      // ...
    },

    styles() {
      // ...
    },
  },

  methods: {
    handleInput() {
      // ...
    },

    handleChange() {
      // ...
    },
  },
};
```

## 单文件组件顶级标签的顺序

单文件组件应该总是让顶级标签的顺序保持一致，且标签之间留有空行。

```html
<template>
  ...
</template>

<script>
  /* ... */
</script>

<style>
  /* ... */
</style>
```