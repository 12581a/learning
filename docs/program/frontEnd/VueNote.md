1. Vue特点

   采用组件化开发，提高代码复用率

   声明式编码，无需操作DOM

   虚拟DOM

2. 插值、指令

3. 数据代理：通过vm对象代理data对象中属性的操作，更加操作data中的数据

4. 事件处理：事件的基本使用、事件修饰符、键盘事件。

5. 计算属性：借助于Object.defineProperty方法，效率更高。更多get，不set。它是一个值。

   get函数什么时候执行：初次读取时执行一次，当依赖的数据发生变化时会被再次调用。

   计算属性最终会出现在vm上，直接读取使用即可。如果计算属性被修改，必须去写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

6. 监视属性：深度监视，监视多级结构中属性的变化，开启deep = true属性即可。现在的写法都是简写形式。computed属性可以完成的事情，watch都可以完成。

   watch能完成的事情，computed不一定能完成，例如，使用watch可以进项异步操作。

   两个重要原则：被Vue管理的函数，最好写成普通函数。不被Vue所管理的函数（定时器的回调函数、ajax的回调函数、Promise的回调），最好写成箭头函数，这样this的指向才是vm或组价实例对象。

7. 绑定样式、条件渲染、列表渲染。遍历数组和对象数据时的key。​

   虚拟DOM中key的作用：key是虚拟DOM对象的标识，当状态中的数据发生变化时，Vue会根据新数据生成新的虚拟DOM。

   用index作为key可能会引发的问题。

   开发中如何选择key:最好使用每天数据的唯一标识作为key,比如id、手机号、学号等唯一值，如果不存在对数据的逆序添加、逆序删除等操作，仅用于列表展示，使用index作为key也是没有问题的。

8. Vue列表排序、过滤。

9. Vue监测数据改变的原理：

   Vue会监视data中所有层次的数据。

   如何监测：通过setter实现监测，在new Vue()时传入要监测的对象。对象中后添加的属性，Vue默认不做响应式处理。如果需要给后添加的属性做响应式，可以使用Vue.set或者vm.$set。

   如何监测数组中的数据：通过包裹数组更新元素的方法实现，本质就是做了两件事：调用原生对应的方法对数组进行更新，重新解析模板，进而更新页面。

   在Vue修改数组中的某一个元素时一定要用如下方法：使用这些API，push()、pop()。

   注意：Vue.set()和vm.$set()不能给vm或者vm的根数据对象添加属性。

10.  v-model在表单中的使用

    在使用文本框的时候，可以不加value属性。在使用单选和复选的时候，要使用添加value属性，其中复选框data接收时要改为[]接收，如果不用[]接收，默认得到的是一个是否选中的boolean值。

    修饰符的使用:lazy、trim

11.   过滤器

     {{ message | capitalize }} 插值语法或者v-bind

12.   内置指令

    v-text：替换节点的内容，{{}}不会、v-html、v-clock：使用css配合v-clock可以解决网速慢时页面展示出{{xxx}}的问题、v-once、v-pre:跳过节点的编译过程、

13.   自定义指令

    局部指令new Vue(directives:{指令名：配置对象}) 全局指令Vue.directive(指令名：{配置对象})

    配置对象中常用的回调：bind、inserted、update

14.  生命周期

    mounted:页面呈现的是经过Vue编译的DOM，对DOM的操作均有效，至此初始化过程结束，一般在此进行开启定时器、发送ajax请求、订阅消息等操作

    beforeDestroy:清除定时器、解绑自定义事件

15.  组件

    局部注册与全局注册

    组件名

    组件的嵌套

    组件本身是一个名为VueComponent的构造函数，不是程序员定义的，是Vue.extend生成的。每次调用Vue.extend，返回的都是一个VueComponent

    关于this指向，在组件中指向【VueComponent】实例对象。而在new Vue()中，this是【Vue】实例对象。

    一个重要的内置关系：VueComponent.prototype.`__proto__` === Vue.prototype
    
    业务组件之间的拆分
    
    组件之间的通信
    
16.   脚手架

    全局安装、创建项目

    babel将ES6转成ES5

    npm run serve运行项目

    render函数

    脚手架的配置：vue-config.js

17.    ref标签属性

    给子组件注册引用信息，访问子组件

18.  props配置

    让组件接收外部传过来的数据

    props属性是只读的，如果进行了修改，就会发出警告。若业务需求要修改，复制props中的内容到data中一份，然后去修改data中的数据。

    props适用于父组件与子组件之间的通信、子组件与父组件之间的通信：要求父组件传给子组件一个函数。

    使用v-model时：v-model的值不能是props传过来的值，应为props是不可以修改的。

19.   mixins配置

    公用的配置提取成一个混入对象。包括全局混入Vue.mixin(XXX)和局部混入mixins:['XXX']

20.   插件

    用于增强Vue，本质是一个包含install方法的一个对象，install的第一个参数是Vue,第二个是插件使用者传递的数据

21.   Scoped样式，让样式在局部内生效，防止生效

22.  浏览器本地存储

    localStorage

23. 组件自定义事件

    一种组件之间通信的方式，适用于子组件给父组件传递数据。有两种方式，一种是<Demo @test="thing"/>，或者是在父组件之中定义<Demo ref="demo">，

    触发自定义事件，使用this.$emit('test', 数据)，组件上也可以绑定原生DOM事件，需要使用native修饰符。

    注意： 通过this.$refs.son.$on('test', 回调)绑定自定义事件时，回调要么配置在methods中，要么使用箭头函数，否则this指向会出现问题

24.  全局事件总线

    任意组件之间通信

25.  消息订阅与发布

    任意组件之间的通信

26. this.$nextTick(回调函数)，作用是在下一次DOM更新结束后指定其指定的回调

    当数据改变之后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

27.  过渡与动画

28.  解决浏览器跨域问题

29.  插槽

    默认插槽、具名插槽

    作用域插槽：数据在组件自身、但根据数据生成的结构需要组件的使用者来决定。

30.  vuex

    多个组件依赖于同一个状态、来自不同组件的行为变更同一个状态

    mapstates:映射state中的属性为计算属性

    mapGetters:映射getters中的数据为计算属性

    mapActions:帮助我们生成与actions对话的方法，即：包含$store.dispatch(xxx)的函数

    mapMutations:帮助我们生成与mutations对话的方法，即：包含$store.commit(xxx)的函数

    * mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递参数，否则参数是事件对象

31.    路由

    每个组件都有一个$route属性，里面存储着自己的路由信息

    整个应用只有一个router，可以通过组件的$router属性获取

    嵌套路由、路由传参、命名路由

    路由的Params参数，在配置路由时，声明接收params参数.路由携带params参数时，若使用to的对象语法，则不能使用path配置项，必须使用name配置

    使用props属性可以给子路由组件传递参数。若props=true，若为真，就会把路由收到的所有params参数，以props的形式传递给子组件。

    router-link中有replace属性，替换当前记录

32.    编程式路由导航

    不借助于router-link实现路由导航，让路由跳转更加灵活

33.   缓存路由组件

    让不展示的路由组件保持挂载，不被销毁

34.   路由组件独有的生命周期

    activated、deactivated

35.   路由守卫：对路由权限进行控制

    全局前置路由、全局后置路由

    router.beforeEach

    独享路由守卫beforeEnter

    组件内路由守卫：组件内配置beforeRouterEnter、beforeRouteLeave

    hash模式与history模式

36.  UI组件库

**Vue3**

1. 使用vue-cli构建项目，使用`vue create vue3`命令创建项目，直接使用`vue run serve`启动服务，还可以使用vite创建项目。

2. setup函数的返回值若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用。也可以返回一个渲染函数。setup不能是一个async函数。

   在beforeCreate之前执行一次，this是undefined

   注意setup的参数以及属性，props：包含组件外部传递过来的，并且组件内部声明接收了的属性。

   context：上下文对象。attrs:值为对象，包含那些组件外部传递过来的，但没有在props属性中申明接收的属性，相当于`this.$attrs`，

   slots:收到的插槽内容，相当于`this.$slots`，emit：分发自定义事件的函数，相当于`this.$emit`

3. ref函数：它的作用是定义一个响应式的数据。基本类型的数据仍然是靠Object.defineProperty()的get与set完成的。对象类型的数据使用reactive函数完成的。

4. reactive函数：定义一个对象类型的响应式的数据。`const res = reactive(obj)`接受一个对象或数组，返回一个代理对象。内部基于ES6的Proxy实现，通过代理对象内部数据进行操作。

5. vue3响应式的原理：

   使用vue2对对象属性进行添加或者删除时，不会监听到，可以使用`this.$set(obj,属性,value)`，`this.$delete(obj,属性)`进行操作。

   直接通过下标修改数组，界面不会自动更新。

   vue3使用Proxy代理拦截任意属性的变化，通过Reflect对被代理对象进行操作。

6. reactive对比ref函数

7. computed属性

   immediate，确认是否以当前的初始值执行handler的函数 

   deep：是否开启深度监视

8. watch属性

9. watchEffect函数

10. 生命周期函数

11. 自定义hook函数

    组合API，类似于mixin。自定义hook：复用代码，让setup中的逻辑更加清楚。

12. toRef()函数

    - 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。
    - 语法：`const name = toRef(person,'name')`
    - 应用: 要将响应式对象中的某个属性单独提供给外部使用时。
    - 扩展：`toRefs` 与`toRef`功能一致，但可以批量创建多个 ref 对象，语法：`toRefs(person)`

13. 其他组合API

    shallowReactive：只处理对象最外层属性的响应式（浅响应式）

    shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理

    什么时候使用?

    如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive
    如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef

    readonly: 让一个响应式数据变为只读的（深只读）。
    shallowReadonly：让一个响应式数据变为只读的（浅只读）。
    应用场景: 不希望数据(尤其是这个数据是来自与其他组件时)被修改时。

    toRaw：
    作用：将一个由reactive生成的响应式对象转为普通对象。
    使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
    markRaw：
    作用：标记一个对象，使其永远不会再成为响应式对象。
    应用场景:
    有些值不应被设置为响应式的，例如复杂的第三方类库等。
    当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

14. customRef

    创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。 

15. provide与inject

    实现祖与后代组件间通信

16. 异步引入组件defineAsyncComponent

Vue项目

1. 函数的防抖与节流

   节流：在规定的时间间隔内不会重复触发回调，只有大于这个回调间隔才会触发回调，把频繁触发变为少量触发。

   防抖：前面的所有触发都被取消，最后一次执行在规定时间之后才会触发，也就是说连续快速的触发，只会执行一次。

2. mockjs模拟数据

3. swipper轮播图插件使用

4. async与await使用

5. nextTick

6. 分页：pageNo第几页、pageSize：每一页展示多少条数据、total：总数据条数、continues：分页连续页码个数

7. 校验vee-validate

8. 路由懒加载

​    



后台管理系统



​    

​    

​    

​    

​    

​    

​    

​    

​    

​    

​    

​    



​    

​    

​    

​    

​    

   































