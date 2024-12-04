## 一、选项式 API

### 1.1 什么是选项式 API

**选项式 API**（Options API）的设计对象是基于对象的，将 <font color="blue">**Vue 实例的各个部分拆分成不同的选项**</font>，并在创建 Vue 实例时将这些作为选项传入。常用的选项包括 `data`、`methods`、`computed`、`watch` 等。

组件的功能通过一组对象配置来组织，每个选项都有特定的用途。

```javascript
Vue.component('my-component', {
  data() {
    return {
      message: 'Hello, Vue!'
    };
  },
  methods: {
    changeMessage() {
      this.message = 'Hello, Composition API!';
    }
  }
});
```



### 1.2 选项式 API 的缺点

在小型项目或简单组件中，选项式 API 的结构直观且易于理解。但是，对于复杂项目，选项式 API 将会遇到以下问题：

- **可读性**：当组件的逻辑变得复杂时，代码会变得难以维护和理解。由于数据和逻辑被分散在多个选项中，很难一眼看出它们之间的关系。

- **复用代码**：对于复用逻辑代码也存在一定的困难，因为逻辑代码往往与特定的data和methods紧密耦合。





## 二、组合式 API

### 2.1 什么是组合式 API

为了解决选项式 API 暴露出来的问题，Vue3 提出了 组合式 API。

组合式 API 通过函数和 `setup()` 函数来组织逻辑。<font color="red">`setup()` 函数是组件的入口</font>，用于声明响应式数据、计算属性、方法等。组合式 API 允许将相关逻辑封装成函数，以便重用。

```javascript
import { ref } from 'vue';

export default {
  setup() {
    const message = ref('Hello, Vue!');
    
    const changeMessage = () => {
      message.value = 'Hello, Composition API!';
    };

    return {
      message,
      changeMessage
    };
  }
};
```



### 2.2 具体案例

(1) 使用选项式 API

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue 示例</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
</head>
<body>
  <div id="app">
    <h2>Todo List</h2>
    <ul>
      <li v-for="task in tasks">{{ task }}</li>
    </ul>
    <input type="text" v-model="newTask" placeholder="Enter a new task">
    <button @click="addTask">Add Task</button>
  </div>

  <script>
    new Vue({
      el: '#app',
      data: {
        tasks: ['Learn JavaScript', 'Build a project'],
        newTask: ''
      },
      methods: {
        addTask() {
          if (this.newTask.trim() !== '') {
            this.tasks.push(this.newTask);
            this.newTask = '';
          } else {
            alert('Please enter a task.');
          }
        }
      }
    });
  </script>
</body>
</html>
```

(2) 使用组合式 API

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue 示例</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@3"></script>
</head>
<body>
  <div id="app">
    <h2>Todo List</h2>
    <ul>
      <li v-for="task in tasks" :key="task">{{ task }}</li>
    </ul>
    <input type="text" v-model="newTask" placeholder="Enter a new task">
    <button @click="addTask">Add Task</button>
  </div>

  <script  type="module">
   import { createApp, ref } from 'vue'

    createApp({
      setup() {
        // 使用 ref 创建响应式数据
        const tasks = ref(['Learn JavaScript', 'Build a project']);
        const newTask = ref('');

        // 添加任务的方法
        const addTask = () => {
          if (newTask.value.trim() !== '') {
            tasks.value.push(newTask.value);
            newTask.value = '';
          } else {
            alert('Please enter a task.');
          }
        };

        // 返回数据和方法，以便模板访问
        return {
          tasks,
          newTask,
          addTask
        };
      }
    }).mount('#app');
  </script>
</body>
</html>
```



## 三、选项式 API vs 组合式 API

组合式 API 是 Vue3 为了解决 选项式 API 的缺点特意推出来的，在使用方式上也有相应区别。



### 3.1 响应式数据

**选项式 API**：通过 `data()` 返回对象来定义响应式数据。

```javascript
data() {
  return {
    count: 0
  };
}
```

**组合式 API**：使用 `ref()`（对于基本数据类型）或 `reactive()`（对于对象）来创建响应式数据。

```javascript
import { ref } from 'vue';

setup() {
  const count = ref(0);
  return { count };
}
```





### 3.3 生命周期钩子

- **选项式 API**：生命周期钩子作为对象的选项定义，如 `created`、`mounted`、`updated` 等。

  ```javascript
  mounted() {
    console.log('Component is mounted!');
  }
  ```

- **组合式 API**：生命周期钩子作为 `onX` 函数调用，`onMounted`、`onBeforeMount`、`onUpdated` 等。

  ```javascript
  import { onMounted } from 'vue';
  
  setup() {
    onMounted(() => {
      console.log('Component is mounted!');
   });
  }
  ```





### 3.2 逻辑复用

**选项式 API**：逻辑复用通常通过混入（mixins）或继承来实现，但这可能导致命名冲突和不易维护的代码。

**组合式 API**：逻辑复用更直观，可以通过自定义组合函数（composables）来提取和共享逻辑。组合函数是普通的 JavaScript 函数，它们可以返回响应式数据和方法，允许你按功能组织代码。

```javascript
// composable.js
import { ref } from 'vue';

export function useCounter() {
  const count = ref(0);
  const increment = () => count.value++;
  return { count, increment };
}

// Component.vue
import { useCounter } from './composable';

export default {
  setup() {
    const { count, increment } = useCounter();
    return { count, increment };
  }
};
```





## 参考资料

[响应式 API：核心 | Vue.js](https://cn.vuejs.org/api/reactivity-core.html)

[Vue3中的组合式API与选项式API：深入理解与比较_vue3选项式和组合式区别-CSDN博客](https://blog.csdn.net/u010362741/article/details/137670597)

