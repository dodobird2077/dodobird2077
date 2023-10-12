
B站地址和语雀笔记: 
```js
// 视频地址
https://www.bilibili.com/video/BV1Zy4y1K7SH/?spm_id_from=333.337.search-card.all.click\&vd_source=b620a69473c31bfeacc5e3bc26991cb3

// 笔记
https://www.yuque.com/cessstudy/kak11d/hfeef2
```

    12.22
    P1-P4 初识Vue
    P5-P6 第一个demo, 模板语法
    P7-P8 模板语法, 数据绑定
    P9 el和data的写法
    P11-P12  数据代理  defineProperty 感觉有点像魔术方法, 进一步是用一个数来修改另外一个数
    P14-P15 事件处理, 事件修饰符
    P16-P17 键盘事件
    P18-19-20 - 计算属性(computed) 对比直接用methods, 区别是
    属性就是data里面的内容, 计算属性就是拿data里面的属性去加工 (感觉就是写在computed中的属性)
    P21-22 监视属性(watch)
    23 深度监视 - watch deep 监视多级结构中属性的变化
    24-25 简写,computed对比watch: 用computed更简单(一般情况下), 但watch能开启异步
    P26-27  绑定class样式- 动态的类写在 :class 中, 可以用变量,数组,对象存储
    P28 条件渲染 - v-show, v-if, v-else-if,v-else  (一组v-if, else 之类的, 中间不能空行), template
    P29-P30 列表渲染 <li v-for="(item,index) in list" :key="index">{{item.name}}</li>
    key的原理: 虚拟DOM对比法, 用key做新旧DOM的对比, 来决定是否复用
    P31-列表过滤(搜索过滤) ①.使用watch实现, filter, indexOf, immediate, 以及一个临时的数组filterList
    P32-列表排序 sort
    P33-   数据监测
    34 数据监测的原理(正在学习 27分钟 )
    35- vue.set()方法  Vue.set(vm.student, 'sex', 男')
    36 数组监测
    37 数据监测总结  (07:43) 有点像魔术方法
    P38- 收集表单数据
    P39-过滤器 (10:00)  filters(和methods同级) 像ERP框架的模板方法
    P40- V-text指令
    41 V-html (有关xss攻击, cookie问题)
    42 v-cloak (阻塞导致页面闪现的问题, 显示原始标签 {{name}})
    43 v-once 只渲染一次,就变成静态数据了
    44 v-pre 让vue调过该节点, 不解析
    P45 自定义指令:  directives 和methods同级 , 模板更新时就会调用
    46 对象式  {bind(), inserted() 元素被插入页面时执行, updated()}
    47 总结
    P48 生命周期  mounted(挂载) vue完成模板解析时调用
    49 挂载流程 beforeCreate created beforeMonuted mounted
    50 更新流程 beforeUpdate updated
    51 销毁流程 beforeDestory  destiryed
    52 总结
    P53 组件化编程
    54 非单文件组件
    55 组件的注意点
    56 组件嵌套
    57 VueComponent 构造函数 组件的本质是一个函数, 调用Vue.extend的时候返回的函数组件的this也是 VueComponent,简称vc, 而不是vm
    58 Vue实例和组件实例
    59  一个重要的内置关系   prototype 原型属性  感觉和继承差不多  Vc实例对象->Vc原型对象->Vue原型对象->Object原型对象

    P60 单文件组件  <template><script><style>
    P61 脚手架   安装: npm install -g @vue/cli, 创建 vue create demo01, 运行, npm run serve
    P62  分析脚手架的目录结构:
    P63  render 函数 模板解析器 (性能原因,用于精简vue.js)
    P64  修改默认配置
    P65 ref属性  类似html的id, 用 this.$ref.RefName. 组件通信的时候用
    P66 props配置  组件传值
    P67 mixin混入  类似于公共函数 , ES的暴露需要了解一下
    P68 插件 src/plugins.js  类似于包含公共函数, 公共过滤器之类的全局文件, 增强Vue
    P69 Scoped 样式 (作用域)  css样式, 用于解决类名冲突 <style scoped>
    P70-P77 TodoList 案例  组件化编码步骤(通用)
    P70 静态\
    P71 初始化列表
    P72 添加 兄弟组件传值, 通过共同的父组件来通信 , 父组件定义一个函数传给子组件, 然后子组件调用函数实现传值给父组件
    P73 勾选  祖组件->父组件->孙组件
    P74  删除 filter
    P75  底部统计 reduce统计
    p76 底部交互  v-model
    p77 总结  - 完结
    P78 浏览器本地存储 localStorage.setItem('name', '张三'), getItem('name'), removeItem(),clear() => 微信端是 wx.setStorage?
    sessionStorage.setItem('name', '张三'), 其他方法和上面相同,浏览器一关就消失
    P79 Todolist 本地存储
    P80 组件的自定义事件-绑定  子组件传值给父组件 $this.emit('MyFun', value)
    P81 解绑  this.$off('MyFun')
    P82 自定义事件总结
    P83 Todolist 自定义事件
    P84 全局事件总线  GlobalEventBus 任意组件间通信, 一个全局工具人
    P85 全局事件总线 公交车 $bus, 在创建vue时beforeCreate用: Vue.prototype.$bus = this,
    在使用时接收数据方:mouted中: this.$bus.$on('xxx', methodsName), 发送数据方: this.$bus.emit('xxx', 数据)
    P86 Todolist使用$bus
    P87 消息订阅发布 pubsub.js 第三方库, 安装: npm i pubsub-js, 载入 import pubsub from "pubsub-js", 使用 pubsub.publish('hello', 666)
    P88 TodoList使用消息订阅
    P89  todolist编辑 (写在demo3上)
    P90 $nextTick
    P91  动画效果 过渡和动画 (00:00)  --跳过4节
    P92 过渡效果
    P93 多个元素过渡
    P94 总结过渡和动画
    P96  配置代理 方式一
    P97 配置代理 方式二
    P98 github案例 静态组件
    P99 列表展示
    P100 完善按钮  (00:00)
    P101 vue-resource 一个发送网络请求的, 已停止维护跳过
    P102 插槽
    P103
    P104
    P105 VUEX
    P106 求和案例
    P107 vuex工作原理
    P108 搭建Vuex环境
    p109 求和案例vuex版  dispatch, commit
    p110 Vuex开发者工具使用
    P111  getters配置项 (和action,mutationst同级)
    P112 mapState, mapGetters,  使用方法: 先import {mapState} from 'vuex', 然后在computed中,  ...mapState(\['sum',school']) 映射vuex中的state值
    P113  mapActions, mapMutations
    P114 多个组件共享数据
    P115 Vuex模块化 (vue高级用法)
    p116 Vuex模块化

    P117 路由 VueRouter
    p118 路由的基本使用
    P119 几个注意点
    P120  多级路由
    P121 路由的query参数
    P122 命名路由  name配置项, 别名
    P123 路由params参数  this.$router.params接收
    P124 路由的props配置  用于params接收参数简化写法
    P125 route-link的replace属性,  历史记录相关
    P126 编程式路由导航   this.$router.push
    P127 <keep-alive>标签, 可用于保存表单数据,不销毁  (00:00)
    p128 两个新的生命周期钩子  路由组件激活和失活 activated deactivated()
    P129 路由守卫 前置路由守卫,用于实现权限管理 router.beforeEach
    P130 后置路由守卫
    P131 独享路由守卫(只对一个路由) beforeEnter
    P132 组件内路由守卫
    P133 History模式和Hash模式及项目打包上线  mode:hash   hash就是那个#号(hash模式,默认)
    P134 ElementUI 的基本使用
    \========================================== Vue3 =========================================
    P136 Vue3 简介
    P137  创建项目
    P138 使用Vite创建项目(了解)  npm init vite-app demo01, cd demo01, npm i, npm run dev
    P139 分析Vue3的目录
    P140 安装开发者工具
    P141 初识setup
    P142 ref函数  处理基本类型  让数据响应式 (响应式: 即增删改页面数据时, 自动刷新页面)
    P143 处理对象类型 (00:00) ref()
    P144 reactive 函数 和ref差不多, 比ref更常用, 用于响应式数据, 显示对象比较方便
    P145 回顾vue2的响应式原理, $set , $delete
    P146 Vue3的响应式原理 Proxy  (代理)
    P147 Vue3的响应式原理 Reflect (反射)
    P148 ref对比reactive
    P149 setup的两个注意点, 执行时机很早,this为null, 可传两个参数setup(props, context)
    P150 computed计算属性
    P151 watch监视ref定义的数据
    P152.监视reactive响应式对象, 有些bug需要了解, deep和oldvalue问题
    P153 WATCH时的value问题
    P154 watchEffect函数
    P155 vue3生命周期
    P156 自定义hook(钩子, 就是一个封装的插件)
    P157 toRef和toRefs
    p158-P164 shallowReactive和shallowRef(了解即可)
    P165-P167 新的组件 Fragment(无需了解),Teleport(传送:让组件在指定位置显示),Suspense(还在实验中)组件
    P168 总结, Vue3中的其他改变
