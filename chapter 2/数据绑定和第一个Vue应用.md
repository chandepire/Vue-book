[TOC]

### 数据绑定和第一个Vue应用

示例请参考代码Vue 示例.html

#### 2.1 Vue实例与数据绑定

##### 2.1.1 实例与数据

Vue应用创建很简单，通过Vue的构造函数就可以创建一个Vue的根实例

```html
var app = new Vue({
	// 选项
})
// 变量app就代表了Vue这个实例
// 其中必不可少的一个选项就是el，el用于指定一个页面中已存在的DOM元素来挂在Vue实例，它可以是HTMLELEMENT, 也可以是CSS选择器，比如：
<div id="app"></div>
var app = new Vue({
	el:document.getElementById('app') // 或者是‘#app’
})
// 挂在成功后，我们可以通过app.$el来访问该元素。Vue提供的很多属性和方法都是是$开头的。
```

Vue实例本身业代理了data对象里的所有属性，所以也可以像这样对数据进行访问

```html
var app = new Vue({
	el: '#app',
	data: {
		a:2
	}
})
console.log(app.a)
```

除了可以显示声明数据，也可以指向一个已有的变量，并且他们默认是双向绑定的。

```html
var muData = {
	a: 2
}
var app = new Vue({
	el: '#app',
	data: muData
})
app.a = 12
console.log(muData.a) // 12
```

##### 2.1.2 生命周期

Vue的生命周期钩子有如下这些函数

* created 实例创建后调用，此阶段完成了数据的观测，但是还没有挂载，所以$el还不可用。需要初始化处理一些数据时会比较有用。
* mounted el挂载到实例上后调用，一般我们的第一个业务逻辑会放在该函数中
* beforeDestroy 实例销毁之前调用，主要用于解绑一些使用addEventListener监听的事件等

这些钩子函数都在Vue实例中定义，且它们的this指向的是所在的Vue的实例

```
var app = new Vue({
    el: '#app',
    data: {
        a: 2
    }
    created: function() {
        console.log(this.a)
    },
    mounted: function() {
        console.log(this.$el)
    }
})
```

##### 2.1.3 插值与表达式

大括号{{}}是最基本的文本插值方法，它会自动的将我们双向绑定的数据实时显示出来

```html
<div id="app">
    {{ book }}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            book: 'ad'
        }
    })
</script>
```

实时显示的一个demo

```html
<div id="app">
    {{ date }}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            date: new Date()
        },
        mounted: function() {
            var _this = this;
            this.timer = setInterval(function(){
                _this.date = new Date()
            }, 1000);
        },
        beforeDestroy: function() {
            if (this.timer) {
                clearInterval(this.timer); // 在Vue实例销毁前，清除我们的定时器
            }
        }
    })
</script>
```

如果想要输出html，而不是数据，可以使用v-html

```html
<div id='app'>
    <span v-html="link"></span>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            link: '<a href="#">这是一个链接</a>'
        }
    })
</script>
```

如果想显示{{}}标签，使用v-pre即可以跳过这个元素和它的子元素的编译过程

```html
<spna v-pre>{{ hello world, 这里的内容是不会被编译的}}</spna>
```

{{}}除了简单的绑定属性值，还可以使用Js表达式进行简单的运算、三元运算，并且还只支持单个表达式，不支持语句和流控制。且不能使用用户自定义的全局变量，只能使用Vue白名单内的全局变量，例如Math和Date

```html
{{ number / 10 }}
{{ isOk? '1':'0'}}
{{ text.split('.').reverse().join(',')}}
{{ var book = 'hello'}} // 这是语句
{{ if (ok) return msg }} // 这是流控制
```

#####  2.1.4 过滤器

Vue支持在{{}}插值后面加一个管道符‘|’对数据进行过滤，经常用于格式化文本。过滤的规则是自定义的，通过添加filters进行设置

```html
<div id="app">
    {{ date | formatDate }}
</div>
<script>
    var padDate = function(value) {
        return value < 10 ? '0' + value: value;
    };
    var app = new Vue({
        el: '#app',
        data: {
            date: new Date()
        },
        filters: {
            formatDate: function (value) {
                var date = new Date(value);
                var year = date.getFullYear();
                var month = padDate(date.getMonth() + 1);
                var day = padDate(date.getDate());
                var hours = padDate(date.getHours());
                var minutes = padDate(date.getMinutes());
                var seconds = padDate(date.getSeconds());
                return year + '-' + month + '-' + day + ' ' +hours + ':' + minutes + ':' + seconds;
            }
        },
        mounted: function() {
            var _this = this;
            this.timer = setInterval(function(){
                _this.date = new Date()
            }, 1000);
        },
        beforeDestroy: function() {
            if (this.timer) {
                clearInterval(this.timer); // 在Vue实例销毁前，清除我们的定时器
            }
        }
    })
</script>
```

用法还可以如下所示

```html
{{ message | filterA | filterB }}
{{ message  | filter('arg1', 'arg2')}} // 'arg1', 'arg2'分别是第二和第三个参数
```

#### 2.2 指令与事件

指令的主要职责就是当其表达式的值改变时，相应的将某些行为应用到DOM上，通常带有前缀v-，比如v-if，v-html，v-pre。

v-if可以将元素进行插入和移除

```html
<div id="app">
    <p v-if="show">
        显示这段文字
    </p>
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            show: true
        }
    })
</script>
```

v-bind是动态更新HTML元素上的属性，比如id，class

```html
<div id="app">
    <a v-bind:href="url">链接</a>
    <img v-bind:src="imgUrl">
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            url: 'https://www.github.com',
            imgUrl: 'http://xxx.xxx.xx/img.png'
        }
    })
</script>
```

v-on 用来绑定事件监听器，进行交互功能的开发

```html
<div id="app">
    <p v-if="show">
        这是一段文字
    </p>
    <button v-on:click="handleClose">
        点击隐藏
    </button>
    /**
        <button v-on:click="show=false">
        点击隐藏
    </button>
    **/
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            show: true
        },
        methods: {
            handleClose: function(){
                this.show = false
            }
            /**
            close: function(){
            	this.handleClose()
            }
            **/
        }
    })
</script>
```

#### 2.3 语法糖

语法糖是指在不影响功能的情况下，添加某种方法实现同样的效果，从而方便开发

```html
<a v-bind:href="url"></a>
<a :href="url"></a>

<button v-on:click="handle">
</button>
<button @click="handle">
</button>
```

