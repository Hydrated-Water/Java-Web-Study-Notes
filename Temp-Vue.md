### Vue 概述

[官网](https://cn.vuejs.org/)



### Vue 入门



#### 概述

TODO

渐进式框架



单文件组件：支持在一个 .vue 文件中声明 JavaScript、模版和样式



选项式API与组合式API



```cmd
npm create vue@latest
pnpm create vue@latest
```

```cmd
npm install
pnpm install
```

```cmd
npm run dev
pnpm run dev
```



#### Hello World

创建应用实例，一个应用需要一个根组件

在`index.html`中声明或引用所需的JavaScript

```html
<!DOCTYPE html>
<html lang="">
  ...
  <body>
    <div id="mainApp"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

在JavaScript中声明创建应用实例

```javascript
import { createApp } from 'vue'

const app = createApp(/* 根组件 */);
```

可以从一个单文件组件中导入根组件

声明最简单的`.vue`文件并导入

```vue
<template>
  <div>Hello World!</div>
</template>
```

```javascript
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App);
```

创建的应用需要被挂载到指定“容器”元素下才能渲染，可以通过指定元素或选择器来指定“容器”

```javascript
app.mount('#mainApp');
```

`.mount()` 方法应该始终在整个应用配置和资源注册完成后被调用



`createApp()`方法返回应用实例，而`.mount()`方法返回根组件实例

应用实例和根组件实例对象都有一些属性和方法用于更多的功能



根组件可以在“容器”元素中声明，如

```html
<div id="app">
  <button @click="count++">{{ count }}</button>
</div>
```

```javascript
const app = createApp({
  data() {
    return {
      count: 0
    }
  }
});
app.mount('#app');
```



一个页面中可以存在多个应用实例，这意味这可以调用多次`createApp(...).mount(...)`以在同一个页面中使用多个Vue应用



#### 基本的模板语法

在`App.vue`中声明

```vue
<script>
export default {
  data() {
    return {
      value: 1
    }
  }
}
</script>

<template>
  <div>
    Hello World!
    <div>value = {{value}}</div>
    <button @click="value++">increase the value</button>
  </div>
</template>
```

`{{}}`形式的模板可用于元素内

这将使得`{{value}}`与变量`value`互相绑定，当`value`变量值变更时，`{{value}}`将最新的值以文本的形式渲染到页面上

如果需要渲染HTML而不是文本需要使用`v-html`指令（但这不是推荐的行为）



#### 模板中的表达式

可以在模板语法中使用单一的JavaScript表达式，包括运算符、函数调用等，如

```vue
<div>value = {{ value*2 }}</div>
```

完整的语句、条件控制都不属于表达式，不能在模板中使用

模板中的表达式将被沙盒化，仅能够访问到有限的全局对象列表



#### 属性绑定

如果某元素的某属性值需要动态变化，不应使用`{{}}`模板语法

使用`v-bind:属性名="变量"`的方式将属性值与变量互相绑定

```vue
<script>
export default {
  data() {
    return {
      isHidden: false
    };
  },
};
</script>

<template>
  <div>
    Hello World!
    <button @click="isHidden = !isHidden">
      显示/隐藏按钮
    </button>
    <button v-bind:hidden="isHidden">
      按钮
    </button>
  </div>
</template>
```

`v-bind`是一种指令非常常用，因此可以简写为`:属性名="变量"`的形式，如

```vue
<button :hidden="isHidden">
```

在Vue 3.4以上版本，当属性名与变量名相同时，可以进一步简写成`:属性名`的形式

```vue
<button :hidden><!-- 等效为 --><button :hidden="hidden">
```



一些属性（如`disabled`）没有值，可以通过将其绑定到布尔型变量上，Vue将自动决定该属性存在与否

```vue
<button :disabled="isButtonDisabled">Button</button>
```



可以通过`v-bind="对象变量"`的形式，将多个属性通过对象的字段名绑定，如

```javascript
data() {
  return {
    objectOfAttrs: {
      id: 'container',
      class: 'wrapper'
    }
  }
}
```

```vue
<div v-bind="objectOfAttrs"></div>
```



#### 指令

指令是一类以`v-`开头的属性，它们的声明格式大多为`v-指令名:参数="值"`，如果指令没有参数，格式可能为`v-指令名="值"`，一些指令可能包含修饰符，其格式可能为`v-指令名:参数.修饰符="值"`



#### 指令的参数

参数不是必须的，如`v-if`指令（`v-if` 通过一个布尔值决定插入/移除该元素）不需要参数：`v-if="isSeen"`

参数一般是普通文本字符串，如果需要在参数中使用JavaScript表达式，需要将其用`[]`包裹

其中表达式的结果应是一个字符串或`null`（表示不使用该指令）

```javascript
data() {
  return {
    attrName: "title",
    attrValue: "按钮提示"
  };
}
```

```vue
<button v-bind:[attrName]="attrValue">button</button>
```

在简写时也支持，如

```vue
<button :[attrName]="attrValue">button</button>
```

受限于HTML属性命名的语法，在动态参数中的表达式因符号限制难以使用复杂的功能，并且当动态参数声明在HTML内嵌模板而不是`.vue`组件中时，其表达式因大小写不敏感将可能产生潜在问题



#### 指令的简写

部分指令支持简写，如`v-bind:参数="值"`可以简写为`:参数="值"`，`v-on::参数="值"`可以简写为`@参数="值"`



#### 内置指令

[参考文档](https://cn.vuejs.org/api/built-in-directives)

- v-text 更新元素的文本内容
- v-html 更新元素的 innerHTML
- v-show 基于表达式值的真假性，来改变元素的可见性
- v-if 基于表达式值的真假性，来条件性地渲染元素或者模板片段
- v-else 表示 v-if 或 v-if / v-else-if 链式调用的“else 块”
- v-else-if 表示 v-if 的“else if 块”以用于可以进行链式调用
- v-for 基于原始数据多次渲染元素或模板块
- v-on 给元素绑定事件监听器
- v-bind 动态的绑定一个或多个 attribute，也可以是组件的 prop
- v-model 在表单输入元素或组件上创建双向绑定
- v-slot 用于声明具名插槽或是期望接收 props 的作用域插槽
- v-pre 跳过该元素及其所有子元素的编译
- v-once 仅渲染元素和组件一次，并跳过之后的更新
- v-memo 缓存一个模板的子树
- v-cloak 隐藏尚未完成编译的 DOM 模板，在没有构建步骤的环境下需要使用



#### 基本的响应式

在`.vue`组件的`<scrip>`标签中以如下语法声明

```vue
<script>
export default {
  // data 选项用于声明组件的响应式状态，它是一个函数并需要返回一个对象表示该组件的所有状态
  data() {
    // 此对象的所有顶层属性都会被代理到组件实例 this 上，并仅在实例首次创建时被添加，不应在 data 选项外向该对象或 this 中添加需要用于响应式的属性
    return {
      // 避免在顶层属性上使用 $ 或 _ 前缀
      count: 1
    }
  },
  // methods 选项用于向组件添加若干个方法，这些方法将被添加到组件实例 this 上
  methods: {
      // 避免使用箭头函数，这将导致 this 无法正确指向
      increment() {
          // this 指向当前组件实例
          this.count++;
      }
  }
  // 这是一个生命周期钩子
  mounted() {
    // this 指向当前组件实例
    console.log(this.count);
    // 访问状态
    this.count = 2;
    // 访问方法
    this.increment();
  }
}
</script>

<template>
  <!-- 引用组件的状态和方法 -->
  <button @click="increment">{{ count }}</button>
</template>
```

在Vue3中数据基于JavaScript代理实现，它可能造成以下情况：

```javascript
mounted() {
  const newObject = {};
  this.someObject = newObject;
  console.log(newObject === this.someObject); // false
}
```



#### 深层响应性

状态是深度响应的。这意味着当改变嵌套对象或数组时，这些变化也会被检测到

```javascript
data() {
  return {
    obj: {
      nested: { count: 0 },
      arr: ['foo', 'bar']
    }
  }
}
```



#### DOM的更新时机

修改了响应式的状态后DOM不会立即同步更新，而是在`next tick`中更新一次

要等待 DOM 更新完成后再执行额外的代码，可以使用`nextTick()`全局 API

```javascript
methods: {
  async increment() {
    this.count++
    await nextTick()
    // 现在 DOM 已经更新了
  }
}
```



#### 避免有状态的方法

在`methods`选项中使用的方法应是无状态的（不使用自己的状态），因为组件可能被重用

因此需要将状态以某种方式创建于当前组件实例中，例如在`created()`生命周期钩子选项中创建



#### 计算属性

在`computed`选项中定义一个或多个方法，这些方法与`methods`中的方法类似，但会由Vue缓存结果以避免同一个方法的多次调用

这些方法称为计算属性

一个计算属性依赖若干个响应式状态，当响应式状态变化时，计算属性才会被重新调用

计算属性可以作为一个根据其他响应式状态变化而变化的状态在模版中使用



#### 条件渲染

使用指令`v-if`、`v-else`和`v-else-if`来实现根据特定的变量值的变化来决定某元素是否渲染

```vue
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

`v-if`、`v-else`和`v-else-if`可以用于`<template>`标签

`v-show`通过控制样式的`display`属性来决定某个标签是否显示，因此与`v-if`不同，它一定会渲染到DOM树

```vue
<h1 v-show="ok">Hello!</h1>
```

`v-show`不可以用于`<template>`标签，也无法和`v-else`、`v-else-if`共同使用



#### 列表渲染

使用指令`v-for`可以实现通过指定的数组来渲染若干个元素，当源数组变化时（数组对象被替换或数组的若干个元素发生了改变），这若干个元素也将变化

```javascript
data() {
  return {
    items: [1]
  };
},
methods: {
  increaseItems(){
    this.items[this.items.length] = this.items.length + 1;
  }
}
```

```vue
<button @click="increaseItems">increase the items</button>
<ul>
  <li v-for="item in items">{{item}}</li>
</ul>
```

这里需要用到类似迭代器的`元素变量别名 in 源数组变量名`的语法，如果需要访问索引，可以使用如下形式

```vue
<li v-for="(item,index) in items">index: {{index}}, item: {{item}}</li>
```

`v-for`可以嵌套使用，如

```vue
<li v-for="item in items">
  <span v-for="childItem in item.children">
    {{ item.message }} {{ childItem }}
  </span>
</li>
```

`v-for`可以遍历对象，可以使用的语法有：

```vue
<li v-for="value in myObject">{{ value }}</li>
```

```vue
<li v-for="(value, key) in myObject">{{ key }}: {{ value }}</li>
```

```vue
<li v-for="(value, key, index) in myObject">{{ index }}. {{ key }}: {{ value }}</li>
```

`v-for`可以直接遍历整数（从1开始），如

```vue
<span v-for="n in 10">{{ n }}</span>
```

`v-for`可以用于`<template>`标签



当`v-for`引用的数组发生改变时，Vue默认采用“就地更新”的策略，即Vue不会移动DOM元素的顺序，而是仅更改元素的内容，这可以提高性能

但如果一个DOM元素有自己的状态（如表单输入），这可能造成非预期的问题，此时可以通过`v-bind:key="变量"`的语法绑定一个`key`元素属性到指定的变量，Vue将基于这个属性来唯一地标识每个元素，如

```vue
<button @click="this.items.reserve()">reverse the items</button>
<div>items:</div>
<ul>
  <li v-for="item in items" :key="item.id">
    {{item.text}}
    <input type="text" />
  </li>
</ul>
```

`v-bind:key`要求的值类型为字符串或数字

推荐在尽可能的情况下为`v-for`提供`key`元素属性



如果需要对数组进行处理，可以使用方法或计算属性，如

```vue
<li v-for="n in filterArray">{{ n }}</li>
```

```vue
<li v-for="n in fileter(arr)">{{ n }}</li>
```



不建议在同一个元素上同时使用`v-for`和`v-if`，这可能导致非预期的优先级问题



#### 事件处理

使用`v-on`指令可以为元素添加事件处理，语法为`v-on:事件="事件处理器"`，可以简写为`@事件="事件处理器"`

事件处理器包括：

- 内联事件处理器
- 方法事件处理器

内联事件处理器是直接声明的JavaScript语句，如

```vue
<button @click="count++">Add 1</button>
<button @click="increaseCount(1)">Add 1</button>
```

方法事件处理器是可以引用的一个方法，通过在`methods`选项中声明并作为事件处理器引用

```javascript
methods: {
  increaseCount(){
    this.count++;
  }
}
```

```vue
<button @click="increaseCount">Add 1</button>
```

方法事件处理器的方法可以接受一个`event`事件对象，这个对象默认情况下为原生DOM事件对象

```vue
methods: {
  increaseCount(event){
    this.count++;
  }
}
```



#### 表单输入

对于表单输入（如`input`标签），可以通过`v-model`属性将输入的内容与指定的变量互相绑定

```vue
<input type="text" v-model="inputText">
<div>inputText: {{inputText}}</div>
```

它与以下写法的功能相同

```vue
<input :value="inputText" @input="event => inputText = event.target.value">
<div>inputText: {{inputText}}</div>
```



`v-model`适用于多种类型的表单输入，如`input`、`textarea`、`select`、`checkbox`、`radio`等等

对于文本类型的`<input>`和`<textarea>`，`v-model`会绑定`value`属性并监听`input`事件

对于`checkbox`和`radio`类型的`<input>`会绑定`checked`属性并监听`change`事件

对于`<select>`会绑定`value`属性并监听`change`事件

这些元素初始声明的`value`、`checked`、`selected`将被忽略



单选框示例

```vue
<input type="radio" value="One" v-model="picked" />
<input type="radio" value="Two" v-model="picked" />
```

可以不声明相同值的`name`属性，`v-model`会自动的处理单选



复选框示例

复选框将绑定布尔类型的值

```vue
<input type="checkbox" id="checkbox" v-model="checked"/>
```

可以将多个复选框绑定到同一个数组上

```javascript
export default {
  data() {
    return {
      checkedValues: []
    }
  }
}
```

```vue
<input type="checkbox" value="value1" v-model="checkedValues" />
<input type="checkbox" value="value2" v-model="checkedValues" />
<input type="checkbox" value="value3" v-model="checkedValues" />
```



选择器示例

```vue
<select v-model="selected">
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
```

多值选择器需要绑定到数组类型的变量上

```vue
<select v-model="selectedArray" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
```

可以和`v-for`共同使用

```vue
<select v-model="selected">
  <option v-for="option in options" :value="option.value">
    {{ option.text }}
  </option>
</select>
```



修饰符

`v-model`支持修饰符：

- `.lazy` 默认情况下`<input>`监听`input`事件，可以通过此修饰符改为监听`change`事件，如

  ```vue
  <input v-model.lazy="msg" />
  ```

- `.number` 将输入通过`parseFloat()`转换为数字类型的值，如果失败则返回原值，在`<input>`的`type="number"`时该修饰符默认启用

  ```vue
  <input v-model.number="age" />
  ```

- `.trim` 去除用户输入内容的两端空格



#### 侦听器

使用`watch`选项可以创建对响应式状态的侦听器，侦听器是指定的响应式状态值变化时的回调方法，用于当响应式状态发生变化时触发自定义的逻辑

一个侦听器可以接收两个参数表示侦听的变量变化的新值和旧值

```vue
<script>
export default {
  data() {
    return {
      value: 1,
      valueString: ""
    };
  },
  watch: {
    value(newValue, oldValue) {
      this.valueString = `value change from ${oldValue} to ${newValue}`;
    }
  }
};
</script>

<template>
  <div>
    <div>{{ valueString }}</div>
    <button @click="value++" >increase the value</button>
  </div>
</template>
```

在`watch`选项中使用方法名指定对应的响应式变量，如果需要侦听嵌套的响应式变量，可以使用ES 6的计算属性名语法

```javascript
watch: {
  'obj.nest.key'(newKey, oldKey) {
    ...
  },
  ['obj.nest.value']: function(newValue, oldValue){
    ...
  }
}
```



侦听器可以通过组件实例的`$watch()`方法动态地创建，这个方法将返回一个函数，用于停止对应的侦听器

```javascript
const unwatch = this.$watch('foo', callback);
...
// 停止该侦听器
unwatch();
```



一个侦听器可以有若干个选项，按照如下的方式声明

```javascript
export default {
  watch: {
    someVar: {
      handler(newValue, oldValue) {
        ...
      },
      deep: true,
      immediate: true,
      once: true,
      flush: 'post'
    }
  }
}
```

- deep

  一个侦听对应变量的侦听器只侦听该变量本身的变化，如果这个变量是个对象且其若干嵌套层次的属性发生变化，默认情况下不会触发侦听器

  使用该选项并设值为`true`可改变该行为

- immediate

  一个侦听器仅当对应的变量变化时才被调用，使用该选项并设值为`true`可使得一个侦听器在创建后立即执行（在`created`钩子之前）

- once

  使用该选项并设值为`true`可使该侦听器只会被调用一次

- flush

  使用该选项可以改变回调触发的时机，默认情况下触发时机在当前组件的DOM更新之前

  `post`值意味着在DOM更新之后触发回调

  `sync`值意味着在 Vue 进行任何更新之前触发回调



#### 模板引用

使用`ref`属性可以将指定的DOM元素绑定到指定的引用上，以用于访问底层DOM元素

在元素上使用`ref="引用名"`进行绑定，随后通过组件实例的`$refs.引用名`进行访问

```vue
<script>
export default {
  mounted() {
    this.$refs.inputElement.focus();
  }
}
</script>

<template>
  <input ref="inputElement" />
</template>
```

模版引用只有在组件挂载后才能使用，在那之前引用的值是`undefined`，这意味着不能在模板语法中使用模板引用



可以在`v-for`的元素上使用`ref`，此时对应的引用的值类型为数组，但其并不保证与源数组相同的顺序



可以在`:ref="内联函数或组件方法"`的形式使其在每次组件更新时都被调用，该函数可接受一个参数表示DOM元素

```vue
<input :ref="(el) => { ... }">
```



可以在组件上使用`ref`



#### 基本的组件

一个Vue应用实例由一个或若干个组件实例组成，其中至少需要一个根组件

![组件树](https://cn.vuejs.org/assets/components.B1JZbf0_.png)

以下是一个简单的例子，它将一个根组件挂载到原生HTML页面的指定元素下

`index.html`

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="UTF-8">
    <title>App</title>
  </head>
  <body>
    <div id="mainApp"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

`src/main.js`

```javascript
import SimpleComponent from './SimpleComponent.vue'

const app = createApp(SimpleComponent).mount('#mainApp');
```

`src/SimpleComponent.vue`

```vue
<template>
  <div>
      Hello World
  </div>
</template>
```



通常在`.vue`文件中定义组件，即单文件组件 (简称 SFC)

这允许在一个组件中声明模板、逻辑和样式

示例

```vue
<script>
export default {
  data() {
    return {
      greeting: 'Hello World!'
    }
  }
}
</script>

<template>
  <p class="greeting">{{ greeting }}</p>
</template>

<style>
.greeting {
  color: red;
  font-weight: bold;
}
</style>
```

一个单文件组件需要构建后才能工作，以完成将模板语法编译成可执行的逻辑等步骤，如果不使用构建，可以使用如下的定义方式：

```javascript
export default {
  data() {
    return {
      greeting: 'Hello World!'
    }
  },
  template: `
    <p class="greeting">{{ greeting }}</p>`
}
```

或者在原生HTML的DOM内直接编写模版，但这将受HTML语法的限制，易造成诸多问题



一个组件可以有若干个子组件，以实现组件的复用

在一个组件中使用另一个组件需要将其导入并声明到当前组件的选项`components`中，随后可以在模板中以标签的形式声明一个或若干个组件

`SimpleComponent.vue`

```vue
<script>
import SimpleSubComponent from "./SimpleSubComponent.vue";

export default {
  components: {
    SimpleSubComponent: SimpleSubComponent,
  },
};
</script>

<template>
  <div>
    Hello World
    <ul>
      <SimpleSubComponent />
      <SimpleSubComponent />
      <SimpleSubComponent />
    </ul>
  </div>
</template>
```

`SimpleSubComponent.vue`

```vue
<script>
export default {
  data() {
    return {
      value: 1,
    };
  },
};
</script>
<template>
  <li>a item value = {{ value }} <button @click="value++">value+1</button></li>
</template>
```

多次声明同一个组件将创建多个组件实例，不同的组件实例之间不会相互影响



#### 组件的注册

组件的注册有两种方式：

- 全局注册
- 局部注册

通常采用局部注册的方式，通过`components`选项，为特定父组件注册所需的指定的子组件

```javascript
import ComponentA from './ComponentA.vue'
import ComponentB from './ComponentB.vue'
import ComponentC from './ComponentC.vue'

export default {
  components: {
    ComponentA, // ES2015 简写语法，等效于 ComponentA: ComponentA
    ComponentB,
    ComponentC
  }
}
```

上述注册方式仅对当前组件有效，如果需要为全局注册一个指定的组件，可以使用Vue应用实例的`.component()`方法

```javascript
app.component(
  // 注册的名字
  'MyComponent',
  // 组件的实现
  {
    /* ... */
  }
);
```

```javascript
x10 1import ComponentA from './ComponentA.vue'

app.component('ComponentA', ComponentA);
```



推荐以大驼峰命名的方式为组件命名，以与普通HTML标签区分开来



#### 组件的生命周期

每个组件都将经历如创建、挂载、卸载等生命周期，可以为组件声明这些生命周期的钩子函数，以作为生命周期的回调

![组件生命周期图示](https://cn.vuejs.org/assets/lifecycle_zh-CN.W0MNXI0C.png)

使用生命周期钩子作为方法名声明组件的选项，如

```javascript
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```



#### 组件的Props

组件实例与组件实例之间是隔离的，如果一个父组件实例需要向它的子组件实例单向传递数据，并且还希望子组件实例能够响应该数据的变化，可以使用 props

子组件需要在`props`选项中以字符串数组的方式声明若干个 props，随后这些 props 可以像组件属性（响应式状态）一样使用

`SimpleSubComponent.vue`

```vue
<script>
export default {
  props: ['valueFromParent']
};
</script>

<template>
  <div>
    valueFromParent = {{valueFromParent}}
  </div>
</template>
```

父组件需要以属性`props名称="值"`的方式在组件标签上声明，随后属性值将被赋值给子组件的指定 props

`SimpleComponent.vue`

```vue
<SimpleSubComponent valueFromParent="foobar"/>
```

父组件可以将其绑定到变量上，这样子组件对应的 props 的值的变化将受父组件的逻辑控制

`SimpleComponent.vue`

```vue
<script>
import SimpleSubComponent from "./SimpleSubComponent.vue";

export default {
  data(){
    return {
      value: 0
    }
  },
  components: {
    SimpleSubComponent: SimpleSubComponent,
  },
};
</script>

<template>
  <div>
    <button @click="value++">value+1</button>
    <ul>
      <SimpleSubComponent :valueFromParent="value"/>
      <SimpleSubComponent :valueFromParent="value"/>
      <SimpleSubComponent :valueFromParent="value"/>
    </ul>
  </div>
</template>
```

对于子组件，它的 props 是只读的，并且也不推荐以任何可能的方式向 props 中写入，这可以保证 props 数据的单向传递



props 的数据只会从父组件实例传递到子组件实例，如果需要跨越多层嵌套组件传递，需要多次声明



#### 组件的事件

如果一个子组件实例需要向其父组件实例传递数据，可以通过组件事件的方式

组件的事件名称是自定义的，并推荐在组件的`emits`选项中以字符串数组的方式显示声明这个组件可能触发的所有组件事件

一个子组件实例可以通过组件实例的`$emit()`方法触发一个组件事件，这个方法的第一个参数为自定义事件的名称，其后可以携带0或若干个参数用于提供给监听器

`SimpleSubComponent.vue`

```vue
<script>
export default {
  emits: ['subComponentMonuted','noticeValue'],
  data() {
    return {
      value: 1,
    };
  },
  mounted(){
    this.$emit('subComponentMonuted');
  }
};
</script>

<template>
  <div>
    <li>a item value = {{ value }} <button @click="value++">value+1</button></li>
    <button @click="$emit('noticeValue',value,'other param1','other param2')">notice parent</button>
  </div>
</template>
```

需要接收子组件事件的父组件，可在其声明的子组件标签上，以`v-on:子组件事件名="回调"`或`@子组件事件名="回调"`的简写形式捕获子组件的事件，其中回调函数可以声明若干个参数以接受来自子组件`$emit()`中传递的参数

`SimpleComponent.vue`

```vue
<script>
import SimpleSubComponent from "./SimpleSubComponent.vue";

export default {
  components: {
    SimpleSubComponent: SimpleSubComponent,
  },
  methods:{
    onNoticeValue(value, p1, p2){
      console.log(value,p1,p2);
    }
  }
};
</script>

<template>
  <div>
    <ul>
      <SimpleSubComponent @subComponentMonuted="console.log('subComponentMonuted')"/>
      <SimpleSubComponent @noticeValue="onNoticeValue"/>
      <SimpleSubComponent/>
    </ul>
  </div>
</template>
```



组件事件只会从子组件实例触发并在父组件实例捕获，如果需要跨越多层嵌套组件传递，需要多次触发



#### 状态管理

props 和组件事件都有其局限性，如单向传递、无法跨越多层、声明繁琐等，如果需要在许多组件中共享一个响应式的状态，可采取其他方法

一个简单的方式是使用`vue`模块的`reactive()`方法在一个独立的模块中创建一个响应式状态变量，随后在其他组件中可以导入该响应式状态

`store.js`

```javascript
import {reactive} from 'vue'

export const globalValue = reactive({
    name: 'global value',
    value: 0,
    resetValue(){
        this.value = 0;
    }
})
```

`SimpleSubComponent.vue`

```vue
<script>
import {globalValue} from './store.js'

export default {
  data() {
    return {
      globalValue
    };
  }
};
</script>

<template>
  <div>
    globalValue = {{globalValue.value}}
    <button @click="globalValue.value++">globalValue+1</button>
    <button @click="globalValue.resetValue()">reset value</button>
  </div>
</template>
```



另一个办法是使用如 Pinia 这样的状态管理方案



#### 组件的透传属性

父组件在声明子组件标签时，在标签上声明了一些属性，但这些属性不是 props 也不是组件事件的监听器，那么它们将会被渲染到子组件的根元素上



#### 组件的插槽

如果希望父组件在声明子组件标签时将指定的模板渲染到子组件内，可以使用插槽功能

在子组件中声明`<slot>`标签以作为插槽出口

`SlotComponent.vue`

```vue
<template>
  <div class="square">
    <slot></slot>
  </div>
</template>
```

父组件声明子组件标签时可向标签内声明任意模板内容，包括普通文本、一个或多个标签，甚至是组件，即插槽内容

`ParentComponent.vue`

```vue
<template>
  <div>
    Hello World
    <SlotComponent>
        {{message}}
        <button>Hello World</button>
        <SlotComponent />
    </SlotComponent>
  </div>
</template>
```

值得注意的是，插槽内容中的模板不能访问子组件实例的数据，这是因为模板在父组件中声明，只能访问父组件



可以在插槽出口内声明默认内容，默认内容将当父组件没有声明任何插槽内容时运用

```vue
<template>
  <div class="square">
    <slot>{{defaultMessage}}</slot>
  </div>
</template>
```



可以在一个组件中声明多个插槽，并为这些插槽命名，其中默认的插槽名称为`default`

```vue
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

在父组件中使用`<template v-slot:插槽名>`或`<template #插槽名>`的简写语法指定插槽，未使用`<template>`标签指定的插槽内容属于默认插槽

与普通的指令参数相同，这里也支持通过`[]`指定动态参数

```vue
<BaseLayout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #[footerSlotName]>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```



子组件可以通过类似 props 的方式向插槽出口声明 props 属性，父组件通过声明`v-slot="变量名"`从而在插槽内容中以响应式状态的方式访问 props

`SlotComponent.vue`

```vue
<template>
  <div class="square">
    <button @click="value++">value+1</button>
    <slot message="Hello World" :value="value">{{defaultMessage}}</slot>
  </div>
</template>
```

`ParentComponent.vue`

```vue
<template>
  <div>
    <SlotComponent>
        <template v-slot="defaultProps">
            message = {{defaultProps.message}}, value = {{defaultProps.value}}
        </template>
    </SlotComponent>
  </div>
</template>
```



#### 路由

客户端路由，即客户端的JavaScript拦截页面的跳转请求，动态获取新的数据，然后在无需重新加载的情况下更新当前页面，以带来更顺滑的用户体验

可以通过JavaScript监听浏览器`hashchange`事件来原生地实现客户端路由，也可以使用[Vue Router](https://router.vuejs.org/zh/)



#### Element Plus

[Element Plus](https://element-plus.org/zh-CN/)是一个基于Vue 3的组件库，可以用于简化开发
