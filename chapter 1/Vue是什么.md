[TOC] 初识Vue.js

### 1.1 Vue是什么

> Vue.js 是一个用于创建用户界面的开源JavaScript框架，也是一个创建单页应用的Web应用框架。它简单小巧（Vue.js压缩后大小仅有17KB），渐进式（可以一步一步有阶段的使用Vue.js）技术栈，足以应付任何规模的应用。并且还提供了Web开发中常见的高级功能，比如：
>
> * 解耦视图与数据
> * 可复用的组件
> * 前端路由
> * 状态管理
> * 虚拟dom

#### 1.1.1 MVVM模式

> MVVM模式是由经典的软件架构MVC衍生而来。即当View(视图层)变化时，会自动更新到ViewModel（视图模型），view和ViewModel之间通过双向绑定的建立的联系
>
> View <------> ViewModel <-------> Model

#### 1.1.2 Vue.js有什么不同

> Vue.js通过MVVM的模式拆为视图与数据两部分，并将其分离。因此，你只需要关心你的数据即可。

### 1.2 如何使用Vue

> 先对传统前端开发模式和Vue.js的开发模式做一个对比，以此链接Vue.js产生的背景和核心思想

#### 1.2.1 传统的前端开发模式

> 传统前端解决方案：
>
> jQuery + RequireJs + artTemplate + Gulp
>
> 由于这套技术方案的简单、高效、实用，至今仍有不少开发者在使用。不过随着项目的扩大和时间的推移，出现了更复杂的业务场景，比如SPA（单页面富应用）、组件解耦。为了提升开发效率，降低成本，这时就出现了Angular、React和Vue.js

#### 1.2.2 Vue.js的开发模式

> Vue.js是一个渐进式的JavaScript框架，如果想简单的开发页面，可以直接通过script加载CDN文件
>
> ```html
> <!-- 自动识别最新稳定版本Vue.js>
> <script src="https://unpkg.com/vue/dist/vue.min.js"></script>
> <!-- 制定某个具体版本的Vue.js>
> <script src="https://unpkg.com/vue@2.1.6/dist/vue.min.js"></script>
> ```
>
> 具体代码参考Vue-示例.html

> 对于业务逻辑复杂，对前端工程有要求的项目，可以使用Vue单文件的形式配合webpack，还可以使用vuex来管理状态，vue-router来管理路由