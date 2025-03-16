# vue学习

## 初始化项目

1. 创建文件夹
2. 终端 用cd命令进入想要创建的示例的文件夹
3. 执行vue create moz-todo-vue
4. 选择默认选项 Default （[Vue 3] babel,eslint) 然后按下Enter 

## 项目结构

- `package.json`：该文件包含项目的依赖项列表，以及一些元数据和 `eslint` 配置。

- `babel.config.js`：这个是 [Babel](https://babeljs.io/) 的配置文件，可以在开发中使用 JavaScript 的新特性，并且将其转换为在生产环境中可以跨浏览器运行的旧语法代码。你也可以在这个里配置额外的 babel 插件。

- `jsconfig.json`：这是一份用于 [Visual Studio Code](https://code.visualstudio.com/docs/languages/jsconfig) 的配置文件，它为 VS Code 提供了关于项目结构的上下文信息，并帮助自动完成。

 `public`：这个目录包含一些在 [Webpack](https://webpack.js.org/) 编译过 程中没有加工处理过的文件（有一个例外：index.html 会有一些处理）。

- `favicon.ico`：这个是项目的图标，当前就是一个 Vue 的 logo。
- `index.html`：这是应用的模板文件，Vue 应用会通过这个 HTML 页面来运行，也可以通过 lodash 这种模板语法在这个文件里插值。

- **`src`：这个是 Vue 应用的核心代码目录**
  - **`main.js`：这是应用的入口文件。目前它会初始化 Vue 应用并且制定将应用挂载到 `index.html` 文件中的哪个 HTML 元素上。通常还会做一些注册全局组件或者添额外的 Vue 库的操作。**
  - **`App.vue`：这是 Vue 应用的根节点组件，往下看可以了解更多关注 Vue 组件的信息。**
  - **`components`：这是用来存放自定义组件的目录，目前里面会有一个示例组件。**
  - **`assets`：这个目录用来存放像 CSS、图片这种静态资源，但是因为它们属于代码目录下，所以可以用 webpack 来操作和处理。意思就是你可以使用一些预处理比如 [Sass/SCSS](https://sass-lang.com/) 或者 [Stylus](https://stylus-lang.com/)。**

   ### .vue文件（单文件组件）

#### App.vue

看到由 `<template>`、`<script>` 和 `<style>` 三部分组成，分别包含了组件的模板、脚本和样式相关的内容。所有的单文件组件都是这种类似的基本结构。

`<template>` 包含了所有的标记结构和组件的展示逻辑。template 可以包含任何合法的 HTML，以及一些我们接下来要讲的 Vue 特定的语法。

<script> 包含组件中所有的非显示逻辑，最重要的是，<script> 标签需要默认导出一个 JS 对象。该对象是你在本地注册组件、定义属性、处理本地状态、定义方法等的地方。在构建阶段这个包含 template 模板的对象会被处理和转换成为一个有 render() 函数的 Vue 组件。

## 创建组件

1. 在你的`moz-todo-vue/src/components`目录下，创建一个`ToDoItem.vue`的新文件。在你的代码编辑器中打开该文件。
2. 通过在文件顶部添加`<template></template>`来创建组件的模板部分。
3. 在你的模板部分下面创建一个`<script></script>`部分。在`<script>`标签内，添加一个默认导出对象`export default {}`，这是你的组件对象。

export default{} 用于将组件内的对象数据 暴露出去

## 在应用程序中使用组件

1. 再次打开`App.vue`文件。

2. 在`<script>`标签的顶部，添加以下内容来引入`ToDoItem`组件：

   ```
   import ToDoItem from "./components/ToDoItem.vue";
   ```

3. 在你的组件对象里面，添加 `components` 属性，然后在它里面添加你的 ToDoItem 组件进行注册。

```javascript
import ToDoItem from "./components/ToDoItem.vue";

export default {
  name: "app",
  components: {
    ToDoItem,
  },
};
```

实际展示组件 需要在模板中使用元素<to-do-item></to-do-item>**组件文件名及其在 JavaScript 中的表示方式总是用大写驼色（例如 `ToDoList`），而等价的自定义元素总是用连字符小写（例如 `<to-do-list>`）。**

## props让组件动态化

可以让数据发生变化 ，单向传递，只能从父级向子组件中传递数据

### 注册props

```
<script>
  export default {
    props: {
      label: { required: true, type: String },
      done: { default: false, type: Boolean }
    }
  };
</script>
```

### 使用已注册的props

`{{}}` 是 Vue 中的一个特殊的模版语法，它能在模版内打印类中定义的 JavaScript 表达式的结果，包括值和方法。重要的是，`{{}}` 里的内容是作为文本显示，而非 HTML。在此例中，我们打印的是 `label` 的值。

```javascript
<template>
  <div>
    <input type="checkbox" id="todo-item" checked="false" />
    <label for="todo-item">{{label}}</label>
  </div>
</template>
```

需要在App.vue中给prop赋值

如

```
<to-do-item label="My ToDo Item"></to-do-item>
```

## data属性

```
data() {
  return {
    key: value
  }
}
```

可以双向传递数据

this 可以从内部数据访问组件的props和其他属性

```
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
  },
  data() {
    return {
      isDone: this.done,
    };
  },
};
```

如何把isDone 属性附加到我们的组件呢？

和{{}}类似

v-bind： 简写 ：

```javascript
<input type="checkbox" id="todo-item" v-bind:checked="isDone" />

<input type="checkbox" id="todo-item" :checked="isDone" />
```

## 利用 v-for 指令渲染列表

首先需要准备一个代办事项数组。添加data属性到App.vue组件对象中，它包含一个ToDoItems字段，其值是待办事项数组。每个待办事项用一个对象表示，这个对象含有label和done属性

```
export default {
  name: "app",
  components: {
    ToDoItem,
  },
  data() {
    return {
      ToDoItems: [
        { label: "Learn Vue", done: false },
        { label: "Create a Vue project with the CLI", done: true },
        { label: "Have fun", done: true },
        { label: "Create a to-do list", done: false },
      ],
    };
  },
};
```

现在我们有了列表，接下来可以用v-for展示他们了

v-for=“item in items”——items是你要迭代的列表

item是数组中当先元素的引用

## key属性

在即你行数据传递之前，我们要了解下key属性，它和v-for一起使用，用来帮助Vue标识列表中的元素

1.使用导入ToDoItem组件相同的方法导入lodash.uniqueid 到App组件中

```javascript
import uniqueId from "lodash.uniqueid";
```

2.添加id字段到ToDoItems数组的每一个元素中，并赋值为uniqueId('todo-').

```javascript
import ToDoItem from "./components/ToDoItem.vue";
import uniqueId from "lodash.uniqueid";
export default {
  name: "app",
  components: {
    ToDoItem,
  },
  data() {
    return {
      ToDoItems: [
        { id: uniqueId("todo-"), label: "Learn Vue", done: false },
        {
          id: uniqueId("todo-"),
          label: "Create a Vue project with the CLI",
          done: true,
        },
        { id: uniqueId("todo-"), label: "Have fun", done: true },
        { id: uniqueId("todo-"), label: "Create a to-do list", done: false },
      ],
    };
  },
};
```

3.在你的 `App.vue` 模板中添加 `v-for` 指令和 `key` 属性到 `<li>` 元素：

```
<ul>
  <li v-for="item in ToDoItems" :key="item.id">
    <to-do-item label="My ToDo Item" :done="true"></to-do-item>
  </li>
</ul>
```

4.把 `label="My ToDo Item"` 改成 `:label="item.label"`, `:done="false"` 改成 `:done="item.done"`，像下面这样：

htmlCopy to Clipboard

```
<ul>
  <li v-for="item in ToDoItems" :key="item.id">
    <to-do-item :label="item.label" :done="item.done"></to-do-item>
  </li>
</ul>
```

现在App组件中已经给每个待办事项赋予了id

所以不需要在组件中data属性里再赋予id

在这里我们可以做一个小小的重构。我们可以把 `id` 变成一个 prop，而不是在 `ToDoItem` 组件中为复选框生成它。虽然这不是严格意义上的需要，但它使我们更容易管理，因为我们已经需要为每个 todo 项目创建一个唯一的 `id`。

1. 添加一个新的 prop `id` 到 `ToDoItem` 组件。
2. 标记它为 required，类型是 `String`。
3. 为防止命名冲突，删除掉 `data` 属性中的 `id` 字段。
4. 现在不需要再使用 `uniqueId` 了，所以需要删除掉 `import uniqueId from 'lodash.uniqueid';` 这行，否则你的应用会报错。

现在，`ToDoItem` 中的 `<script>` 内容应该如下所示：

```
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
    id: { required: true, type: String },
  },
  data() {
    return {
      isDone: this.done,
    };
  },
};
```

现在，在 `App.vue` 组件中将 `item.id` 作为 prop 传递给 `ToDoItem` 组件。你的 `App.vue` template 应该如下所示：

```
<template>
  <div id="app">
    <h1>My To-Do List</h1>
    <ul>
      <li v-for="item in ToDoItems" :key="item.id">
        <to-do-item
          :label="item.label"
          :done="item.done"
          :id="item.id"></to-do-item>
      </li>
    </ul>
  </div>
</template>
```

你渲染后的站点看起来是没有变化的，但是这次重构使得 `item.id` 像其他参数一样，作为 prop 从 `App.vue` 传递给 `ToDoItem`。现在代码变得更有逻辑性和一致性。

## 使用Vue event、method和model添加一个新的todo表单

### 创建一个新的待办事项表单

1. 在 components 目录下，新建文件 `ToDoForm.vue`。

2. 创建一个空的 `<template>` 和 `<script>`：

   ```
   <template></template>
   
   <script>
     export default {};
   </script>
   ```

3. 新建一个 HTML 表单来允许我们输入新的待办项并把它提交到 app。我们需要一个 <form>,它里面包含一个<label>、一个<input>、一个<button>.更新后的模版如下：

   ```
   <template>
     <form>
       <label for="new-todo-input"> What needs to be done? </label>
       <input
         type="text"
         id="new-todo-input"
         name="new-todo"
         autocomplete="off" />
       <button type="submit">Add</button>
     </form>
   </template>
   ```

   因此，现在我们有一个 form 组件可以用来输入新的待办项的标题，它最终会渲染成 `ToDoItem` 的 label。

4. 我们把这个组件添加到 app 中，返回 `App.vue` 然后在 `<script>` 添加下面的语句：

   ```
   import ToDoForm from "./components/ToDoForm";
   ```

5. 在你的 App 组件中注册它

   ```
   components: {
     ToDoItem, ToDoForm;
   }
   ```

6. 最后将 `ToDoForm` 组件添加到 App 中的 `<template>` 中，像下面这样：

   ```
   <template>
     <div id="app">
       <h1>My To-Do List</h1>
       <to-do-form></to-do-form>
       <ul>
         <li v-for="item in ToDoItems" :key="item.id">
           <to-do-item
             :label="item.label"
             :done="item.done"
             :id="item.id"></to-do-item>
         </li>
       </ul>
     </div
   </template>
   ```

但是，此时点击添加按钮后，页面会将表单发送回服务器，但这不是想要的，我们想要做的是在submit事件上运行一个方法，该方法将添加App中定义的ToDoItem数据列表的新待办事项。为此，我们需要向组件实例添加一个方法

### 创建一个方法并用v-on绑定这个方法到一个事件上

```
<form @submit="onSubmit"></form>
```

submit是事件，onSubmit是方法

但是运行程序时，应用程序依然会将数据发布到服务器，从而导致刷新。因此我们需要阻止事件的默认操作通过页面冒泡

- `.stop`：停止传播事件。等效于常规 JavaScript 事件中的 [`Event.stopPropagation()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopPropagation)。
- `.prevent`：阻止事件的默认行为。等效于 [`Event.preventDefault()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault)。
- `.self`：仅当事件是从该确切元素分派时触发处理程序。
- `{.key}`：仅通过指定键触发事件处理程序。 [MDN 有一个有效键值列表](https://developer.mozilla.org/zh-CN/docs/Web/API/UI_Events/Keyboard_event_key_values); 多词键只需转换为 kebab 大小写（例如 `page-down`）。
- `.native`：监听组件根（最外层的包装）元素上的原生事件。
- `.once`：监听事件，直到它被触发一次，然后不再触发。
- `.left`：仅通过鼠标左键事件触发处理程序。
- `.right`：仅通过鼠标右键事件触发处理程序。
- `.middle`：仅通过鼠标中键事件触发处理程序。
- `.passive`：等效于在 vanilla JavaScript 中使用 [`addEventListener()`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener) 创建事件监听器时传入 `{ passive: true }` 参数。

### 用v-model 来绑定数据到输入

接下来，我们需要一种从表单的 `<input>` 中获取值的方法，这样我们就可以将新的待办事项添加到我们的 `ToDoItems` 数据列表中。

我们首先需要的是表单中的 `data` 属性来跟踪待办事项的值。

1. 向我们的 `ToDoForm` 组件对象添加一个 `data()` 方法，该方法返回一个 `label` 字段。我们可以将 `label` 的初始值设置为空字符串。

   你的组件对象现在应该如下所示：

   ```
   export default {
     methods: {
       onSubmit() {
         console.log("form submitted");
       },
     },
     data() {
       return {
         label: "",
       };
     },
   };
   ```

2. 我们现在需要一些方法将 `new-todo-input` 元素字段的值附加到 `label` 字段。Vue 对此有一个特殊的指令：[`v-model`](https://vuejs.org/v2/api/#v-model)。`v-model` 绑定到你在其上设置的数据属性，并使其与 `<input>` 保持同步。`v-model` 适用于所有不同的输入类型，包括复选框、单选框和选择输入。要使用 `v-model`，你需要向 `<input>` 添加一个结构为 `v-model="variable"` 的属性。

   所以在我们的例子中，我们会将它添加到我们的 `new-todo-input` 字段中，如下所示。现在就这样做：

   ```
   <input
     type="text"
     id="new-todo-input"
     name="new-todo"
     autocomplete="off"
     v-model="label"
   />
   ```

1. 让我们通过记录在我们的 `onSubmit()` 方法中提交的数据的值来测试我们对 `v-model` 的使用。在组件中，使用 `this` 关键字访问数据属性。所以我们使用 `this.label` 访问我们的 `label` 字段。

   更新你的 `onSubmit()` 方法，使其如下所示：

   ```
   methods: {
     onSubmit() {
       console.log('Label value: ', this.label);
     }
   },
   ```

2. 现在回到你正在运行的应用程序，在 `<input>` 字段中添加一些文本，然后单击“添加”按钮。你应该会看到你输入的值已记录到控制台，例如：

   ```
   Label value: My value
   ```

   ### 使用修饰符改变v-model 的行为

   与事件修饰符类似，我们可以添加修饰符来改变v-model的行为。第一个，.trim，将删除输入之前或之后的空格。第二个，.lazy。对于文本输入v-model同步通常使用input事件进行。

   通常这意味着每次输入过程中都在一直同步数据，这显然不是我们想要的，.lazy修饰符导致v-model使用change事件代替，这意味着Vue只会在输入失去焦点或提交表单时同步数据。这显然更合理。

   `v-model.lazy.trim="label"`。

### 使用自定义事件将数据传递给父级

在 `ToDoForm` 的 `onSubmit` 事件中，我们添加一个 `todo-added` 事件。自定义事件的发射方式如下：`this.$emit("event-name")`。重要的是要知道事件处理程序区分大小写并且不能包含空格。Vue 模板也被转换为小写，这意味着 Vue 模板无法监听以大写字母命名的事件。

如何创建事件？

1. 将 `onSubmit()` 方法中的 `console.log()` 替换为以下内容：

   jsCopy to Clipboard

   ```
   this.$emit("todo-added");
   ```

2. 接下来，回到 `App.vue` 并添加一个 `methods` 属性到包含 `addToDo()` 方法的组件对象，如图所示 以下。目前，此方法只需将 `To-do added` 记录到控制台即可。

   ```
   export default {
     name: "app",
     components: {
       ToDoItem,
       ToDoForm,
     },
     data() {
       return {
         ToDoItems: [
           { id: uniqueId("todo-"), label: "Learn Vue", done: false },
           {
             id: uniqueId("todo-"),
             label: "Create a Vue project with the CLI",
             done: true,
           },
           { id: uniqueId("todo-"), label: "Have fun", done: true },
           { id: uniqueId("todo-"), label: "Create a to-do list", done: false },
         ],
       };
     },
     methods: {
       addToDo() {
         console.log("To-do added");
       },
     },
   };
   ```

3. 接下来，将 `todo-added` 事件的事件监听器添加到 `<to-do-form></to-do-form>`，它在事件触发时调用 `addToDo()` 方法。使用 `@` 简写，监听器看起来像这样：`@todo-added="addToDo"`:

   ```
   <to-do-form @todo-added="addToDo"></to-do-form>
   ```

4. 当你提交 `ToDoForm` 时，你应该会看到来自 `addToDo()` 方法的控制台日志。这很好，但我们仍然没有将任何数据传递回 `App.vue` 组件。我们可以通过将额外的参数传递给 `ToDoForm` 组件中的 `this.$emit()` 函数来做到这一点。

   在这种情况下，当我们触发事件时，我们希望将 `label` 数据连同它一起传递。这是通过在 `$emit()` 方法中包含你要作为另一个参数传递的数据来完成的：`this.$emit("todo-added", this.label)`。这类似于原生 JavaScript 事件如何包含数据，除了自定义 Vue 事件默认不包含事件对象。这意味着发出的事件将直接匹配你提交的任何对象。所以在我们的例子中，我们的事件对象只是一个字符串。

   像这样更新你的 `onSubmit()` 方法：

   ```
   onSubmit() {
     this.$emit('todo-added', this.label)
   }
   ```

5. 要真正在 `App.vue` 中获取这些数据，我们需要向我们的 `addToDo()` 方法添加一个参数，其中包含 `label` 新的待办事项。返回 `App.vue` 并立即更新：

   ```
   methods: {
     addToDo(toDoLabel) {
       console.log('To-do added:', toDoLabel);
     }
   }
   ```

### 将新的待办事项添加到我们的数据中

1. 像这样更新你的 `addToDo()` 方法：

   ```
   addToDo(toDoLabel) {
     this.ToDoItems.push({id:uniqueId('todo-'), label: toDoLabel, done: false});
   }
   ```

2. 再次尝试测试你的表单，你应该会看到新的待办事项被附加到列表的末尾。

3. 在我们继续之前，让我们做进一步的改进。如果你在输入为空时提交表单，则没有文本的待办事项仍会添加到列表中。为了解决这个问题，我们可以防止在 name 为空时触发 todo-added 事件。由于 `.trim` 指令已经对 name 进行了修剪，因此我们只需要测试空字符串。回到你的 `ToDoForm` 组件，像这样更新 `onSubmit()` 方法。如果标签值为空，我们就不发出 `todo-added` 事件。

   ```
   onSubmit() {
     if(this.label === "") {
       return;
     }
     this.$emit('todo-added', this.label);
   }
   ```

   ### 使用v-model更新输入值

```
onSubmit() {
  if(this.label === "") {
    return;
  }
  this.$emit('todo-added', this.label);
  this.label = "";
}
```

##  使用CSS样式化Vue组件

Vue具有三种样式化应用程序的方法：

- 外部CSS文件。
- 单文件组件中的全局样式。
- 单文件组件中组件范围的样式

### 外部CSS文件的样式

1. 首先，在 `src/assets` 目录中创建一个名为 `reset.css` 的文件。Webpack 将处理此文件夹中的文件。这意味着我们可以使用 CSS 预处理器（如 SCSS）或后处理器（如 PostCSS）。

2. 接下来，在 `src/main.js` 文件中，如下导入 `reset.css` 文件：

   ```
   import "./assets/reset.css";
   ```

### 向单文件组件添加全局样式

在<style>中添加样式

### 添加作用域样式

我们要添加样式的最后一个组件是我们的 `ToDoItem` 组件。为了使样式的定义靠近组件，我们可以在它里面添加一个 `<style>` 元素。然而，如果这些样式改变了这个组件之外的东西，要追踪到负责的样式并解决这个问题可能会很困难。这就是 `scoped` 属性有用的地方——它为你所有的样式附加了一个独特的 HTML `data` 属性选择器，防止它们在全局范围内发生冲突。

要使用 `scoped` 标识符，在 `ToDoItem.vue` 中创建一个 `<style>` 元素，位于文件的底部，并给它 `scoped` 属性：

```
<style scoped>
  /* … */
</style>
```

## Vue的计算属性

### 使用计算属性

计算属性的工作原理与方法类似，但仅在它们的一个依赖项发生时重新运行。

要创建计算属性，我们需要向组件对象添加一个computed属性，就像我们之前使用的methods属性一样

### 添加摘要计数器

将以下代码添加到 `App` 组件对象中 `methods` 属性的下方。这里的 `listSummary()` 方法将获取已完成的 `ToDoItems` 数量，并返回一个字符串作为摘要的内容。

```javascript
computed: {
  listSummary() {
    const numberFinishedItems = this.ToDoItems.filter((item) =>item.done).length
    return `${numberFinishedItems} out of ${this.ToDoItems.length} items completed`
  }
}
```

我们把{{listSummary}}添加到模版中，现在就可以看到列表的摘要，但是我们没有以任何方式跟踪done的数据，因此已完成项目的数量不会改变

### 追踪done的变化

我们可以为每个复选框附加一个@change事件处理方法

把ToDoItem.vue中的input元素改成以下

```javascript
<input
  type="checkbox"
  class="checkbox"
  :id="id"
  :checked="isDone"
  @change="$emit('checkbox-changed')" />
```

 在App.vue中addToDo()方法下面，添加一个名为updateDoneStatus()的新方法。这个方法应该有一个参数：待办事项的id。我们想要找到与id匹配的项目，并翻转他的done

```
<to-do-item
  :label="item.label"
  :done="item.done"
  :id="item.id"
  @checkbox-changed="updateDoneStatus(item.id)">
</to-do-item>
```

## Vue中的条件渲染：编辑现有的待办事项

### 创建编辑组件

在components 文件夹下创建一个名为ToDoItemEditForm.vue的新文件

### 修改ToDoItem组件

```
<template>
  <div class="stack-small">
    <div class="custom-checkbox">
      <input
        type="checkbox"
        class="checkbox"
        :id="id"
        :checked="isDone"
        @change="$emit('checkbox-changed')" />
      <label :for="id" class="checkbox-label">{{label}}</label>
    </div>
    <div class="btn-group">
      <button type="button" class="btn" @click="toggleToItemEditForm">
        Edit <span class="visually-hidden">{{label}}</span>
      </button>
      <button type="button" class="btn btn__danger" @click="deleteToDo">
        Delete <span class="visually-hidden">{{label}}</span>
      </button>
    </div>
  </div>
</template>
```

接下来定义点击事件处理函数和必要的isEditing标志

```
data() {
  return {
    isDone: this.done,
    isEditing: false
  };
}
```

```
methods: {
    deleteToDo() {
      this.$emit('item-deleted');
    },
    toggleToItemEditForm() {
      this.isEditing = true;
    }
  }
```

### 通过v-if和v-else有条件的显示组件

在ToDoItem组件的根<div>中添加

v-if="!isEditing"



```
<div class="stack-small" v-if="!isEditing"></div>
```

接下来，在该 `<div>` 的关闭标签下面添加下面这行：

```
<to-do-item-edit-form v-else :id="id" :label="label"></to-do-item-edit-form>
```

然后导入和注册ToDoItemEditForm组件

### 退出编辑模式

 1.在ToDoItem组件中的methods里添加一个itemEdited事件向父组件传递数据

```
itemEdited(newLabel) {
  this.$emit('item-edited', newLabel);
  this.isEditing = false;
}
```

2.创建一个editCancelled()方法，将isEditing设置回false

```
editCancelled() {
  this.isEditing = false;
}
```

3.在ToDoItemEditForm组件添加事件处理程序

htmlCopy to Clipboard

```
<to-do-item-edit-form
  v-else
  :id="id"
  :label="label"
  @item-edited="itemEdited"
  @edit-cancelled="editCancelled">
</to-do-item-edit-form>
```

### 更新和删除todo项

在app.vue中添加新方法

```
deleteToDo(toDoId) {
  const itemIndex = this.ToDoItems.findIndex((item) => item.id === toDoId);
  this.ToDoItems.splice(itemIndex, 1);
},
editToDo(toDoId, newLabel) {
  const toDoToEdit = this.ToDoItems.find((item) => item.id === toDoId);
  toDoToEdit.label = newLabel;
```

然后为item-deleted和item-edited事件添加事件监听器

```
<to-do-item
  :label="item.label"
  :done="item.done"
  :id="item.id"
  @checkbox-changed="updateDoneStatus(item.id)"
  @item-deleted="deleteToDo(item.id)"
  @item-edited="editToDo(item.id, $event)">
</to-do-item>## 实现
```

## localstorage 实现

1. **`create` 钩子函数**：在组件创建时调用 `loadToDoItems` 方法，从 `localStorage` 中读取待办事项列表并初始化 `ToDoItems` 数组。
2. **`loadToDoItems` 方法**：从 `localStorage` 中读取名为 `"ToDoItems"` 的数据，并将其解析为 JSON 格式，赋值给 `ToDoItems` 数组。

3. **`saveToDoItems` 方法**：在数据发生变化（添加、更新、删除、编辑待办事项）时调用，将 `ToDoItems` 数组转换为 JSON 字符串并存储到 `localStorage` 中。

4. 在 `addToDo`、`updateDoneStatus`、`deleteToDo` 和 `editToDo` 方法中，每次数据变化后都调用 `saveToDoItems` 方法，确保 `localStorage` 中的数据始终与内存中的数据保持一致。
