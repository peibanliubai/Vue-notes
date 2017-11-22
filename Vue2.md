# VueWeek2
### 生命周期
>Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。

`不能使用箭头函数来定义一个生命周期方法 (例如 created: () => this.fetchTodos())`
生命周期基本过程
```
 let vm = new Vue({ // 根实例，初始化时会调用很多方法（钩子函数）
      beforeCreate(){}, //1. 此方法用不到
      data:{a:1,b:''},
      created(){ // 2.获取ajax，初始化操作
      },
      template:'<div>{{a}}</div>', //4.如果有template属性会用模板替换掉外部html，只要有此属性app中的内容就没有任何意义了，只能有一个根元素，不能是文本节点
      beforeMount(){}, //没有什么实际意义
      mounted(){},// 真实dom渲染完了，可以操作dom了
      beforeUpdate(){ // 一般用watch来替换掉
      },
      updated(){
      },
      beforeDestroy(){ // 可以清除定时器 或者清除事件绑定
          alert('销毁前')
      },
      destroyed(){
          alert('销毁后')
      },
  });
  vm.$mount('#app');//3.要保证有编译的元素
  vm.$destroy();
```
### 生命周期钩子
>初始化时候会调用很多方法,称钩子函数
#### beforeCreate
>组件实例刚被创建,在数据观测 (data observer) 和 event/watcher 事件配置之前被调用;(一般不用)
#### created
>组件实例创建完成,属性已经绑定,但DOM还未生成,挂载阶段还没开始,$el属性还不存在 
`$el:  Vue实例使用的根 DOM 元素`

#### beforeMount
>在挂载开始之前被调用,该钩子在服务器端渲染期间不被调用。没有实际意义
#### mounted
>挂载之后,真实dom渲染完了，可以操作dom了;
>`如果dom元素不是通过v-for循环出来的 只能获取到一个，通过v-for循环出来的可以获取多个`
mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted
`$nextTick:  在修改数据之后立即使用它，然后等待 DOM 更新。回调的 this 自动绑定到调用它的实例上。`
#### beforeUpdate
>数据更新之前调用，发生在虚拟 DOM 重新渲染;
一般会用watch替换掉
#### updated
>数据更新之后调用,一般用计算属性和watch调用;
如果你希望等到整个视图都渲染完毕，也可以用 
vm.$nextTick 替换
#### activated
>keep-alive 组件激活时调用。
#### deactivated
>keep-alive 组件停用时调用。
#### beforeDestroy 
>销毁之前调用,可以清除定时器 或者清除事件绑定
#### destroyed
>销毁之后,Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

```
 额外注意 :template属性
template:'<div>{{a}}</div>', //4.如果有template属性会用模板替换掉外部html，只要有此属性app中的内容就没有任何意义了，只能有一个根元素，不能是文本节点
```
### 实例上的属性方法等
- this.$data     vm上的数据
- this.$watch     监控
- this.$el     当前el元素
- this.$set    后加的属性实现响应式变化
- this.$options    vm上的所有属性
- this.$nextTick(()=>{ // 异步方法，等待渲染dom完成后来获取vm})
- this.$refs     放在dom上 就是获取dom元素的
- $root     当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己

### 组件
>组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素

#### 组件命名
- 1.组件名不要带有大写 多个单词用 -
- 2.只要组件名和定义名字相同是可以的（首字母可以大写）
- 3.html采用短横线隔开命名法 js中转驼峰也是可以的 

#### 全局注册组件
```
Vue.component('myHandsome',{ // 一个对象可以看成一个组件
      template:'<div>{{msg}}</div>',
      data(){// 组件中的数据必须是函数类型 返回一个实例作为组件的数据
        return {msg:'123'}
      }
  });
```
> 一般写插件的时候全局组件使用的多一些
用法： 全局组件 可以声明一次在任何地方使用 局部组件：必须告诉这个组件属于谁

#### 页面级组件
分类 页面级组件：
1.一个页面是一个组件 
2.将可复用的部分抽离出来 基础组件
 >特点:
  提高开发效率
  方便重复使用
  便于协同开发
  更容易被管理和维护
  一个自定义标签 vue就会把他看成一个组件 div p span a header section ...,vue可以给这些标签赋予一定意义
#### 局部组件
局部组件 使用的三部曲 
 - 子组件不能直接使用父组件的数据（组件之间的数据交互）
 - 组件理论上可以无限嵌套
1.创建这个组件
```
let obj = {school:'zfpx'};//如果组件共用了数据 会导致同时更新（独立性）

let component2 = {
        template:'<div>组件2 {{school}} </div>',
        data(){
            return obj
        }
    };
```
2.注册这个组件 
```
let vm = new Vue({
        el:'#app',
        components:{
         component2
        },
        data:{
            a:1
        }
    })
```
3.使用这个组件
```
<component2></component2>
```
组件是相互独立的 不能直接跨作用域 实例也是一个组件,组件中拥有生命周期函数

#### 组件嵌套
>如果要在一个组件使用另一个组件
1.先保证使用的组件得是真实存在
2.在需要引用这个组件的实例上通过components注册这个组件
3.组件需要在父级的模板中通过标签的形式引入
```
let grandson= {
        template:'<div>grandson</div>',
    };
    let son = {
        template:'<div>son <grandson></grandson></div>',
        components:{grandson}
    };
    let parent = {
        template:'<div>parent <son></son></div>',
        components:{
            son
        }
    };
    let vm = new Vue({
        el:'#app',
        template:'<parent><</parent>',
        components:{
            parent
        }
    })
```
#### 组件组合
最常见的就是形成父子组件的关系,组件 A 在它的模板中使用了组件 B。它们之间必然需要相互通信：父组件可能要给子组件下发数据，子组件则可能要将它内部发生的事情告知父组件;这保证了每个组件的代码可以在相对隔离的环境中书写和理解，从而提高了其可维护性和复用性。

>简单理解
`在 Vue 中，父子组件的关系可以总结为 prop 向下传递，事件向上传递。父组件通过 prop 给子组件下发数据，子组件通过事件给父组件发送消息`

组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。父组件的数据需要通过 prop 才能下发到子组件中
```
<div id="app">
    父亲:{{money}}
    <!--当前组件属性:父亲的值,给儿子加一个m属性,属性对应的数据是属于父亲的-->
    <child :m="money"></child>
    <!--带冒号是变量,不带冒号是字符串-->
</div>
```
```
 let vm = new Vue({//parent
        el:'#app',
        data:{money:600},
        components:{
            child:{
                props:{//属性名和data中的名字不能相同,校验时不能阻断代码的指向,只是警告而已
                    m:{//检验属性的类型,如果不带:号,得到的肯定是字符串类型:m="1"
                        type:[String,Boolean,Function,Number,Array,Object],
                        //default:10,//可以给m赋予默认值,如果不传默认调用default;
                       required:true,//此属性是必须传递,但是不能和default同用
                        validator(val){//第一个参数是当前传递的值,返回true表示通过.false不通过
                            return val>200;//自定义校验器(用的不是很多)
                        }
                    }
                },//对象的形式可以进行校验
                //props:["m"],//this.m=100;会在当前子组件上声明一个m属性值是父亲的
                template:'<div>儿子:{{m}}</div>'
            }
        },
    })
```

#### 发布订阅模式
`发布 $emit 订阅  $on`
`又称观察者模式`,定义对象之中一种以一对多的依赖关系,当一个对象的状态发生改变时候,所有依赖于他的对象都将得到通知;广泛用于异步编程中,是一种替代回调函数的方案,订阅一个事件,发生操作A之后,事件发生
```
<div id="app">
    父亲:{{money}}
    <!--child.$on("child-msg",things)-->
    <!--<child :m="money" @update:m="val=>this.money=val"></child>-->
    <child :m.sync="money"></child>
    <!--与上面的一样-->
    <!--写的时候不用,按照用来的方式-->

</div>
<!--父亲绑定一些事件,儿子触发这个事件,将参数传过去,单向数据流,父亲数据刷新,儿子就刷新-->
```
```
let vm = new Vue({
        el:'#app',
        data:{money:400},
        components:{
            child:{
                props:['m'],
                template:'<div>儿子:{{m}} <button @click="getMoney()">多要钱</button></div>',
                methods:{
                    getMoney(){
                        this.$emit("update:m",800);//触发自己的自定义事件让父亲的方法执行
                    }
                }
            }
        }
    })
```
#### slot
用于往具名插槽中插入子组件内容
slot作用 `定制模板`
- 模板中只能有一个根元素，可以通过元素属性定制模板-->
- slot中可以放置一些默认的内容，如果传递了内容则替换掉
- 如果没有名字的标签默认会放置到default中
```
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
```
这是父组件模板
<app-layout>
  <h1 slot="header">这里可能是一个页面标题</h1>
  <p>主要内容的一个段落。</p>
  <p>另一个主要段落。</p>
  <p slot="footer">这里有一些联系信息</p>
</app-layout>
```
```
渲染结果,父组件模板里的内容替换到了上面子组件中对应的内容
<div class="container">
  <header>
    <h1>这里可能是一个页面标题</h1>
  </header>
  <main>
    <p>主要内容的一个段落。</p>
    <p>另一个主要段落。</p>
  </main>
  <footer>
    <p>这里有一些联系信息</p>
  </footer>
</div>
```
#### 组件中父组件调用子组件方法
```
<div id="app">
    <loading ref="load"></loading>
</div>

 // 父组件调用子组件的方法
    let loading = {
          data(){
              return {flag:true}
          },
          template:'<div v-show="flag">疯狂加载中。。。</div>',
          methods:{
              hide(){
                  this.flag = false;
              }
          }
    };
    let vm = new Vue({
        el:'#app',
        mounted(){ // ref 如果放在组件上 获取的是组件的实例 并不是组件的dom元素
              // this.$refs.load.hide()
              // this.$refs.load.$el.style.background = 'red'
        },
        data:{},
        components:{
          loading
        }
    })
```
#### keep-alive
把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染
```
<div id="app">
  <!--template slot transition -->
  <input type="radio" v-model="radio" value="home">home
  <input type="radio" v-model="radio" value="list">list
  <!--一般用作缓存：为的是后面的路由做准备，如果缓存了就不会再走created mounted钩子函数-->

  <keep-alive>
    <component :is="radio"></component>
  </keep-alive>
</div>
```
```
 let home = {
        template:'<div>home</div>',
        mounted(){alert('挂载')},
        beforeDestroy(){alert('销毁')}
    };
    let list = {
        template:'<div>list</div>',
        mounted(){alert('挂载')},
        beforeDestroy(){alert('销毁')}
    };
    let vm = new Vue({
        el:'#app',
        data:{radio:'home'},
        components:{
            home,list
        },

    })
```
#### 平级组件的间的通信
利用EventBus
创建一个新的实例,利用发布订阅模式来促使两个组件之间的通信
```
<div id="app">
  <brother1></brother1>
  <brother2></brother2>
</div>

let EventBus = new Vue;
  let brother1 = {
      template:'<div>{{color}} <button @click="change">变绿</button></div>',
      data(){
        return {color:'绿色',old:'绿色'}
      },
      methods:{
        change(){
            EventBus.$emit('changeGreen',this.old);
        }
      },
      created(){
          EventBus.$on('changeRed',(val)=>{ // 页面一加载兄弟1长个耳朵听
              this.color = val;
          })
      }
  };
  let brother2 = {
      template:'<div>{{color}} <button @click="change">变红</button></div>',
      created(){
          EventBus.$on('changeGreen',(val)=>{
            this.color = val;
          })
      },
      methods:{
        change(){
            EventBus.$emit('changeRed',this.old)
        }
      },
      data(){
          return {color:'红色',old:'红色'}
      }
  };
  let vm = new Vue({
      el:'#app',
      components:{
          brother1,brother2
      }
  })
```

#### 路由
单页Web应用（single page web application，SPA），就是只有一张Web页面的应用，是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序。
>可以配置组件和路由映射,可以使用 Vue.js + vue-router 创建单页应用。
#### vue-router
前端后端分离 后端只负责提供接口前端调用,像跳转都是前端自己处理的,hash模式 # 开发时使用hash 不会导致404 hash的方式不支持seo h5的history.pushState(上线采用h5的跳转)

>默认配好的映射组件会加载到router-view中，可以使用router-link进行页面跳转

>基本结构
```
<div id="app">
    <router-link to="/home" tag="button">首页</router-link>
    <router-link to="/list" tag="button">列表页</router-link>
    <!--router-view是一个全局组件,可以直接使用-->
    <router-view></router-view>
</div>

```
```
 let home={template:"<div>首页</div>"};
    let list={template:'<div>列表页</div>'};
    let routes=[//路由的映射表,配置路径和组件的关系
        {path:'/home',component:home},//配置的关系就是页面级组件,一个页面一个组件
        {path:'/list',component:list}//路径必须加/
        ];
    let router=new VueRouter({
       //引入vue-router自带VueRouter类
        //mode:'history' ,//h5模式
        routes,
        linkActiveClass:'active' //更改默认样式类名,默认叫router-link-active
    });
    let vm=new Vue({
        el:'#app',
        router:router
    })
```

#### 路由参数
vue-router提供了一个$route对象，如果要获取路径传递参,数据会全部存放在params对象中;

>每个组件都会拥有一个名字叫@router(有r的放的都是方法)的属性,还有一个叫$route(没r的放的都是属性)
```
<router-link to="/article/1">文章1</router-link>
<router-link to="/article/2">文章2</router-link>
const routes = [
    {path:'/article/:sid',component:Article}
];
var Article = {
    template:'<div>这是第{{$route.params.sid}}篇文章</div>',
    watch:{
        $route(to,from){
            //to代表要去的路由
            //from代表从哪里来的
            var id = to.params.sid;
            console.log(id);
        }
    }
};
```
`监控路径参数变化使用watch`

#### 路由跳转

`this.$router.push('/list')//强制跳转路径
`          
` this.$router.replace('/list')//路由替换,将当前的历史替换掉
 `
 `this.$router.go(n)//前进/后退n个`
```
 let routes=[
        {path:'',component:home},//默认展示路由
        {path:'/home',component:home},
        {path:'/list',component:list},
        {path:'*',component:list},//这个地方路径不会变,只是切换了组件而已
        {path:'*',redirect:'/home'},//路径变组件也切换  一般404的时候运用
    ];
```
>路径参数:
>article/2/d   匹配出一个对象
  article/:c/:a=>{c:2,a:"d"}=this.$route.params
  拿到对应的

## 模块化
`模块化:是一种将系统分离成独立功能部分的方法,严格定义模块接口,模块间的具有透明性`
>`优点`
>`可维护性:`
>1.灵活结构,焦点分离
>2.方便模块间组合,分解
>3.方便单个模块功能调试,升级
>4.多人协作互不相干
>`可测试性:`
>可分单元测试
>`缺点:`
>性能损耗
>1.系统分层,调用链很长
>2.模块间通信,模块间发送消息会很耗性能

内聚度
内聚度指模块内部实现,他是信息隐藏和局部化概念的自然扩展,它标志着一个模块内部各成分彼此结合的紧密程度.好处很明显,当把相关的任务分组后去阅读就容易多了.设计时应该尽可能的提高模块内聚度,从而获得较高的模块独立性.

耦合度
耦合度则是指模块间的相关联程度的度量.耦合度取决于模块间接口的复杂性,进入或调用模块的位置等.与内聚度相反.再设计时应尽量追求松散耦合的系统


### es6模块化
一个JS文件就是一个模块
>es6模块化:
>import    导入
>在另一个文件中将内容结构出来即可使用
>import拥有声明功能,可以变量提升
import最好放在页面顶部
>export    导出

基本规则或者特点
- 每一个模块只加载一次,每一个js只执行一次,如果下次再去加载同目录下同文件,直接从内存中读取.一个模块就是一个单例,或者说是一个对象
- 每一个模块内声明的变量都是局部变量,不会干扰全局作用域
- 模块内的变量或者函数通过export导出
- 一个模块可以导入别的模块

`如果是第三方模块不需要加./,如果是文件模块必须加./`
>export导出方式
`第一种`
>export{接口},接口名字为定义的变量
>```
let bar="Bar";
let fn=function(){};
export{bar, fn}
import{bar ,fn} from "./list"
>```
>`第二种`
>export{接口 as 名字},可以改变接口名字
>```
>export{bar as foo ,fn as fn1}
>import{foo, fn1}
>```
`第三种`
>export 后直接定义变量,函数
>```
>export let foo=()=>{console.log("foo") ;return "foo"}
>export let bar="Bar"
>import{foo,bar} from "./list"
>```
`第四种`
>export default  直接跟内容不用写变量名
>```
>export default "Foo"
>import foo(变量名承接上面的Foo) from "./list"
>```
`第五种`
>export默认导出函数
>```
>let  fn=()=>""
>export {fn as default}
>import fn1(函数名承接上面函数) from "./list"
>```
`第六种`
>import 接受方式 以*as 变量名 接受全部
>```
>export let a=1;
>export let b=2;
>export let c=()=>{alert("我是c")}
>import *as d from "./list"
>console.log(d.a,d.b);
>d.c()
>```
### node模块化
node模块化的规范是commonjs
每一个node.js文件运行,就会自动创建一个module对象,同时module对象会创建一个exports属性,初始值为{}
`module.exports={}`
导出方式: 
`1.exports.a=1
2.module.exports={a:2}
exports是引用module.exports的值,module.exports被改变的时候,exports不会改变,真正执行导出的是module.exports
`
引入方式:
`require ("./list")`

## vue脚手架(v-cli)
是一种可以帮我们直接生成项目模板的工具
>可以做:
1.目录结构
2.本地调试
3.代码部署
4,热更新
5,单元测试(ESlint)
- 下载一个全局生成vue项目的脚手架

>```
>npm install vue-cli -g
vue init webpack <项目名字>
cd 项目名字
npm install
npm run dev
>```

## webpack
是一种模块加载兼打包的工具,能把各种资源JS(含有JSX),css样式(含less/sass),图片等作为模块来处理
`webpack必须采用commomjs写法`
安装webpack
>```
npm init -y
npm install webpack -save-dev
>```

> 安装webpack或者less最好不要安装全局的，否则可能导致webpack的版本差异
- 在package.json中配置一个脚本，这个脚本用的命令是webpack.会去当前的node_modules下找bin对应的webpack名字让其执行，执行的就是bin/webpack.js,webpack.js需要当前目录下有个名字叫webpack.config.js的文件，我>们通过npm run build执行的目录是当前文件的目录，所以会去当前目录下查找;





## babel转义es6 -> es5
- 安装babel
```
>npm install babel-core --save-dev
npm install babel-loader --save-dev
```

## babel-preset-es2015
- 让翻译官拥有解析es6语法的功能
```
npm install babel-preset-es2015 --save-dev
```

## babel-preset-stage-0
- 解析es7语法的
```
npm install babel-preset-stage-0 --save-dev
```

## 解析样式
- css-loader将css解析成模块，将解析的内容插入到style标签内(style-loader)
```
npm install css-loader style-loader --save-dev
```

## less,sass,stylus(预处理语言)
- less-loader less
- sass-loader
- stylus-loader 
```
npm install less less-loader --save-dev
```

## 解析图片
- file-loader url-loader(是依赖于file-loader的)
```
npm install file-loader url-loader --save-dev
```
## 需要解析html插件
- 插件的作用是以我们自己的html为模板打包后的结果,自动引入到html中产出到dist目录下
```
npm install html-webpack-plugin --save-dev
```
## webpack-dev-server
- 这里面内置了服务,可以帮我们启动一个端口号,当代码更新时候,可以自动打包(内存中打包),代码有变化就重新执行
```
npm install webpack-dev-server --save-dev
```
## 安装vue-loader vue-template-compiler
- vue-loader 解析.vue文件的
- vue-template-compiler 用来解析vue模板的

### 基本写法
#### main.js文件内
```
import './index.css'
import './style.less'
//在js中引入图片需要import,或者写一个线上路径
import page from './1.png'
console.log(page);//page就是打包后的图片路径
let img=new Image();
img.src=page;//写了一个字符串,webpack不会进行查找
document.body.appendChild(img);

import Vue from 'vue';
import App from './App.vue'
//这样直接引用vue 引用并不是vue.js 引用的是vue的runtime
//vue=compiler+runtime(compiler可以编译模板)
//compiler有6k
new Vue({
    //template:'<div>hello</div>'
    //render函数的作用就是将虚拟dom渲染成真实的dom
    //createElement返回的是虚拟的dom
    render:(h)=>h(App)
}).$mount('#app');
/*
render:function (createElement) {
    return createElement('h1',['hello',createElement('span','一则头条')])
}*/
```
#### webpack.config.js文件
```
//webpack必须采用commomjs写法
let path=require('path');//专门处理路径用的,以当前路径解析出一个绝对路径
console.log(path.resolve('./dist'));
 let HtmlWebpackPlugin=require('html-webpack-plugin');
module.exports={
    entry:'./src/main.js',
    //打包的入口文件,webpack会自动查找相关的依赖进行打包
//多个可以用对象 entry:{main:'./src/main.js',main1:'./src/main1.js'}
    output:{
        filename:'bundle.js',//打包后的名字,自己起
        //多个 filename:'[name].js'
        path:path.resolve('./dist')//必须是一个绝对路径
    },
    //模块的解析规则
    //- js 匹配所有的js 用babel-loader转译 排除掉node_module
    module:{
        //use时候从由往左
        //转化base64只在8192字节下转化,其他情况下输出图片
        rules:[{test:/\.js$/,use:'babel-loader',exclude:/node_module/},
            {test:/\.css$/,use:['style-loader','css-loader']},
            {test:/\.less$/,use:['style-loader','css-loader','less-loader']},
            {test:/\.(jpg|png|gif)$/,use:'url-loader?limit=8192'},
            {test:/\.(eot|svg|woff|woff2|wtf)$/,use:'url-loader'},
            {test:/\.vue$/,use:'vue-loader'}
        ]
    },
    plugins:[new HtmlWebpackPlugin({//自动插入到dist目录中
        template:'./src/index.html',//使用的模板
        //filename:'login.html'//产出的文件名字
    })]
};
```
#### .babelrc
```
{
  "presets":["es2015","stage-0"]
}
```





