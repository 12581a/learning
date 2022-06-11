# Vue基础知识

## Vue实例

当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 V。

vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。 

```javascript
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
// 获得这个实例上的 property
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置 property 也会影响到原始数据
vm.a = 2
data.a // => 2
// ……反之亦然
data.a = 3
vm.a // => 3
```

除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 `$`，以便与用户定义的 property 区分开来。例如： 

```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

## 响应式的实现原理

```javascript
// 监测对象改变的原理
    let data = {
        name : "张三",
        age: 18
    }

    let obj = new Observe(data);
    console.log(obj);

    let vm = {};
    vm._data = data = obj;
    console.log(vm);
    
    function Observe(obj){
        const keys = Object.keys(obj);
        keys.forEach((k) =>{
            Object.defineProperty(this, k ,{
                get(){
                    return obj[k];
                },
                set(val){
                    obj[k] = val;
                }
            })
        })
    }
```

## 生命周期

**实例生命周期钩子**

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

## Vue指令

**v-bind 指令**：用于绑定属性

```html
<a v-bind:href="url">百度一下</a>
//可以简写为<a :href="url">百度一下</a>
```

**v-text 指令**：

```html
<span v-text="msg"></span>等价于<span>{{msg}}</span>
```

**v-html 指令**：

双大括号的方式会将数据解释为纯文本，而非HTML。为了输出真正的HTML，可以用v-html指令  

**v-model 指令**： 实现表单输入和应用状态之间的双向绑定。 

```html
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message"> 
</div>  
```

**v-model修饰符** 

.lazy
默认情况下，v-model同步输入框的值和数据。可以通过这个修饰符，转变为在change事件再同步。

```html
<input v-model.lazy="msg">
```

.trim
自动过滤用户输入的首尾空格

```html
<input v-model.trim="msg">
```

**v-cloak 指令**：

能够解决插值表达式闪烁的问题

**v-on 指令**：

主要用来监听dom事件，以便执行一些代码块。 可以给绑定的回调事件传递参数。

```html
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

**事件修饰符** 

> .stop 阻止事件继续传播
> .prevent 阻止默认行为
> .capture 使用事件捕获模式,即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理
> .self 只当在 event.target 是当前元素自身时触发处理函数
> .once 事件将只会触发一次

**事件冒泡**：当一个元素接收到事件的时候 会把他接收到的事件传给自己的父级，一直到window  

**v-if 指令和v-show 指令**：

v-if:每次都会重新创建或删除元素

v-show:只是切换了display样式

```html
<h3 v-if="flag">这是用v-if控制的元素</h3>
<h3 v-show="flag">这是用v-show控制的元素</h3>
```

**v-for 指令**：

基于源数据多次渲染元素或模板块：

```html
<div v-for="item in items">
  {{ item.text }}
</div>
```

另外也可以为数组索引指定别名 (或者用于对象的键)：

```html
<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, name, index) in object"></div>
```

v-for 的默认行为会尝试原地修改元素而不是移动它们。要强制其重新排序元素，你需要用特殊 attribute key 来提供一个排序提示：

```html
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

**自定义指令**

```javascript
/ 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

```javascript
<div id = "app">
	<p v-color='red'>我是红色</p>
</div>

Vue.directive('color', {
	bind : function(el, obj){
		el.style.color = obj.value; 
	}
})
```

 如果想注册局部指令，组件中也接受一个 `directives` 的选项： 

```javascript
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

## 过滤器

Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：**双花括号插值和 `v-bind` 表达式** (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示： 

```javascript
// 定义一个Vue的全局过滤器
Vue.filter('msgFormat', function (msg, arg) {
	return msg.replace(/卑鄙/g, arg);
});

<p>{{msg | msgFormat('高尚')}}</p>
```

局部过滤器

```javascript
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

## Class与Style绑定

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 v-bind 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 v-bind 用于 class 和 style 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

**对象语法**

```html
<div v-bind:class="{ active: true, 'text-danger': false}"></div>
```

还可以写成

```html
<div v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
```

```html
data: {
  isActive: true,
  hasError: false
}
```

绑定的数据对象不必内联定义在模板里：

```html
<div v-bind:class="classObject"></div>
```

```html
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

**数组语法**

我们可以把一个数组传给 v-bind:class，以应用一个 class 列表：

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```html
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

渲染为:

```html
<div class="active text-danger"></div>
```

还可以直接写为：

```html
<div v-bind:class="['active','text-danger']"></div>
```

**style样式绑定**

```html
<p style= "{color : 'red'}">这是个段落</p>
```

## Vue过渡/动画

[链接](https://cn.vuejs.org/v2/guide/transitions.html)

## 组件

这里有一个 Vue 组件的示例： 

```html 
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 `<button-counter> ` 。我们可以在一个通过 `new Vue` 创建的 Vue 根实例中，把这个组件作为自定义元素来使用： 

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```html
new Vue({ el: '#components-demo' })
```

因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 这样根实例特有的选项。

**data 必须是一个函数**,因此每个实例可以维护一份被返回对象的独立的拷贝。

### 组件之间通信的方式

* **父组件向子组件传值**

  在子组件中使用props定义组件的属性，然后在父组件之中绑定属性。

* **子组件向父组件传值（通过自定义事件形式）**

  * 在父组件之中定义一个方法，绑定在子组件之中使用props定义组件的属性，当子组件的属性被触发时，回调执行父组件的方法。

  * 在父组件之中绑定一个事件 `v-on:test='demo'`，事件名是test，在子组件之中触发`this.$emit('test',data)`可以传值，回调函数是在父组件之中的demo。在父组件之中还可以将绑定事件写在`mounted`生命周期方法中，不使用标签的形式，`this.$refs.son.$on('test', this.demo)`。

* **任意组件之间传值**

  * **使用总线的方式**
    * 在发送数据的一方使用`this.$bus.$emit('事件名', data);`，在接受数据的一方使用`this.$bus.$on('事件名', (data) => {})`
  * **使用发布-订阅机制**
    * 导入pubsub-js，发送方`pubsub.publish('事件名', data)`，接收方使用` pubsub.subscribe('事件名',回调函数)`

  * **使用vuex**

### **插槽**

**默认插槽**

在父组件之中使用组件时，`<A><h1>Content</h1></A>`，在组件之中的template之中就可以使用`<slot></slot>`就可以显示内容了。

**具名插槽**

就是给插槽取一个名字，`<A><template slot="center"><h1>Content1</h1></template><template slot="footer"><h1>Content2</h1></template></A>`，在组件之中就可以使用了`<slot name="center"></slot>`，`<slot name="footer"></slot>`

**作用域插槽**

作用域插槽就是子组件可以给父组件传参，父组件决定怎么展示。

在组件中绑定数据`<slot :data="data"></slot>`，在父组件之中就可以展示数据，`<A><template slot-scope="user"><span v-for="item in user.data">{{ item }}</span><template></A>`



# 其他

## nextTick

1. 语法：`this.$nextTick(回调函数)`
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

## mixin

1. 功能：可以把多个组件共用的配置提取成一个混入对象
2. 使用方式:`{data:{...}，methods:{}}`,使用时，全局混入：`Vue.mixin(xxx)` 局部混入：`mixins:['xxx']` 

## WebStorage

浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制。

1. sessionStorage存储的内容会随着浏览器窗口关闭而消失。 
2. localStorage存储的内容，需要手动清除才会消失。 

# Vuex

在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。当多个组件共享数据时使用。

代码示例：

```javascript
// index.js
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex)
const store = new Vuex.Store({
    /*
    state:保存具体的数据
    getters:类似于计算属性
    actions:响应组件中用户的动作
    mutations:执行具体的数据修改
    */
    state:{
        sum:20
    },
    getters:{
        bigSum(state){
            return state.sum * 10;
        }
    },
    actions:{
        add(context, value){
            context.commit('ADD', value);
        }
    },
    mutations:{
        ADD(state, value){
            state.sum += value;
        },
    }
})

```

我们在组件中就可以使用了，还可以利用`...mapMutations`，`...mapActions`

mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。 

```javascript
this.$store.dispatch('add', this.number)
this.$store.commit('ADD' , this.number)
...mapActions['add']
...mapMutations['ADD']
```

还可以直接映射，写在`computed`中，数据就可以直接使用

```javascript
...mapState(['sum'])
...mapGetters(['bigSum'])
```

当数据比较多时，可以采用模块化的写法

```javascript
const store1 = {
  namespaced:true,//开启命名空间
  state:{},
  mutations: { ... },
  actions: { ... },
  getters: {}
}
const store2 = {
  namespaced:true,//开启命名空间
  state:{},
  mutations: { ... },
  actions: { ... },
  getters: {}
}
const store = new Vuex.Store({
  modules: {
    store1,
    store2
  }
})              
```



# Vue-Router

## 编程式路由

不借助`router-link`实现路由跳转，让路由跳转更加灵活 

```javascript
this.$router.push({
	name:'router1',
	params:{
		id:xxx,
		title:xxx
	}
})
this.$router.replace({
	name:'router1',
		params:{
			id:xxx,
			title:xxx
		}
})
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退
```

## 缓存路由组件

让不展示的路由组件保持挂载，不被销毁。 

```javascript
// 缓存About组件-组件名
<keep-alive include="About">
	<router-view></router-view>
</keep-alive>
```

使用编程式路由`$router.push({name: 'home'},query:{},params:{})`时会返回一个Promise对象

## vue路由传值

**query传值**

第一种是to的字符串写法

`router-link :to="/home?id=1001&title=Hello"`

第二种是to的对象写法

`router-link :to="{path:'/',query:{id:1001,title:'Hello'}}"`，如果使用了命名路由，可以将path直接换成`name:''`

**params传值:**

如果想要params参数可传可不传，定义路由时`path:/path/:keywords?`，增加`?`即可

如果params传递的是''，可以使用

字符串写法

`router-link :to="/home/1001/Hello"`，然后在路由配置里面配置`{path:page/:id/:title}`声明接收参数

对象写法

`router-link :to="{name:'/',params:{id:1001,title:'Hello'}}"`，不允许使用path

使用`{{this.$route.query.id}}`或者`{{this.$route.params.id}}`在子路由中接收参数即可

当属性过多时，使用上面的写法读取不可取，所以可以使用`props`

props的第一种写法，在路由规则配置那里`props:{a:1}`，然后在其所对应的路由之中就可以使用`props:['a']`来接收数据

props的第二种写法，在路由规则配置那里`props:true`，就会把该路由组件收到的所有params参数，以props的形式传给该组件 

props的第二种写法，在路由规则配置那里`props($route){return {id:$route.query.id,title:$route.query.title}}`。

**router-link的属性**

* replace: 设置 `replace` 属性的话，当点击时，会调用 `router.replace()` 而不是 `router.push()`，于是导航后不会留下 history 记录。 

## 路由守卫

```javascript
// index.js中
// 全局前置路由守备--初始化被调用/切换路由之前调用
router.beforeEach((to, from, next) =>{
    next();
})
// 后置路由守卫--初始化被调用/切换路由之后调用
router.afterEach((to, from) =>{
   
})
// 配置路由规则时--独享路由守卫
beforeEnter(to,from,next){
}
// 组件内守卫
// 进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {}
// 离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {}
```

路由的两种**工作模式**：hash模式与history模式



# Vue3

## 组合API

### ref与reactive

**ref函数**：定义一个响应式的数据，`const obj = ref(value)`，创建一个包含响应式的数据的引用对象（reference）对象。JS操作数据`obj.value`，模板中读取数据直接使用即可！接收的数据可以是基本类型，也可以是对象类型，基本数据类型的数据响应式仍然是依靠`object.defineProperty()`的get与set完成的。对象类型的数据，仍然是使用reactive函数完成的完成的。

**reactive函数**：定义一个对象类型的响应式数据，返回一个代理对象，它所定义的数据是"深层次"。

### setup

组合式API的入口

所有声明了的 prop，不管父组件是否向其传递了，都将出现在 `props` 对象中。其中未被传入的可选的 prop 的值会是 `undefined` 

**context 解释**

> context.parent === this.$parent
>
> context.root === this
>
> context.emit === this.$emit
>
> context.refs === this.$refs
>
> context.slots === this.slots

案例：

```javascript
import {ref, reactive} from 'vue'
export default {
    setup(props, context){
        let name = ref("zhangsan");
        let person = reactive({
            name: "wangwu",
            age: 99,
            hobby: ["吃饭"]
        }),
        function hello(){
          name.value = "lisi";
          person.name = "王五";
          person.hobby[0] = "吃啥饭";
        }
        // 暴露给模板
        return{
            name,
            person,
            hello
        }
    },
    props:[msg: String]
}
```

setup函数若返回一个对象，则对象里面的属性、方法，在模板中均可直接使用。

若返回一个渲染函数，则可以自定义渲染内容。

### 计算与监听属性

案例

```javascript
setup(props, context){
    let sum = ref(0);
    let person = reactive({
            firstName: "张",
            lastName: "三",
            age: 18
    });
    // 计算属性
    person.fullName = computed(() =>{
        return person.firstName + person.lastName;
    })
    person.fullName = computed({
            get(){
                return person.firstName + person.lastName;
            },
            set(value){}
    });
    // 监听sum属性
    watch(sum,(newValue, oldValue) =>{});
  	// 立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数
    watchEffect(()=>{
        this.sum++;
    })
}
```

## vue3实现响应式的原理

```javascript
let person = {
	name: 'zhangsan',
	age: 18
}; 
const p = new Proxy(person,{
	get(target, propName){
		console.log('读取属性'+ propName);
        // return target[propName];
        Reflect.get(target, propName)
	},
	set(target, propName, value){
		console.log("修改/新增属性" + propName);
		// target[propName] = value;
		Reflect.set(target, propName, value)
	},
	deleteProperty(target, propName){
		console.log("删除属性" + propName);
		// return delete target[propName];
		Reflect.deleteProperty(target, propName)
	}
}) 
```

## 生命周期

```javascript
setup(){
	onMounted(() => {})
    onUpdated(() => {})
    onUnmounted(() => {})
}
```




























