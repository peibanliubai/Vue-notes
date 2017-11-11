# Vue
### 常用的数组变异
#### 操作数组的方法(括号中能改变数组的)
(pop push unshift shift  splice reverse sort) indexOf lastIndexof concat slice
- forEach filter(过滤) map(映射)  some every  reduce  (includes find es6)
- 1. forEach 不支持return
```
arr.forEach(function (item) { // 声明式（不关心如何实现）
    console.log(item);
});
for(let key in arr){ // key会变成字符串类型，包括数组的私有属性也可以打印出来
    console.log(key);
}
let obj = {school:'zfpx',age:8}; //Object.keys将对象的key作为新的数组
//             ['school','age']
for(let val of Object.keys(obj)){ // 支持return 并且是值of数组（不能遍历对象）
    console.log(obj[val]);
}
```

- 2.filter 是否操作原数组： 不  返回结果： 过滤后的新数组   回调函数的返回结果： 如果返回true 表示这一项放到新数组中      (删除)
```
let newAry = [1,2,3,4,5].filter(function (item) {
    return item>2&&item<5;
});
console.log(newAry);
```
- 3.map 映射 将原有的数组映射成一个新数组
```
[1,2,3] <li>1</li><li>2</li><li>3</li>
```
(更新)
 不操作原数组  返回新数组  回调函数中返回什么这一项就是什么
```
let arr1 = [1,2,3].map(function (item) {
    return `<li>${item}</li>`// ``是es6中的模板字符串 遇到变量用${}取值
});
console.log(arr1.join(''));

```

- 4.返回的是boolean
```
let arr3 = [1,2,3,4,5];
console.log(arr3.includes(5));
```
- 5.返回找到的那一项  不会改变数组  回调函数中返回true表示找到了，找到后停止循环,找不到返回的是undefined
```
let result = arr3.every(function (item,index) { // 找到具体的某一项用find

    return item.toString().indexOf(5)>-1
});
console.log(result);
```
- 6.some 找true 找到true后停止 返回true 找不到返回false
- 7.every 找false 找到false后停止 返回false

- 8.reduce 收敛 4个参数 返回的是叠加后的结果 原数组不发生变化，回调函数返回的结果：
`prev`代表的是数组的第一项，`next`是数组的第二项
第二次`prev`是`undefined`,`next`是数组的第三项
```
let sum = [1,2,3,4,5].reduce(function (prev,next,index,item) {
    console.log(prev,next);
    return prev+next;// 本次的返回值 会作为下一次的prev
});
console.log(sum);
```


### vue基础解释
vue数据驱动(主要操作数据)
### 框架和库
- 框架 vue 拥有完整的解决方案 我们写好人家调用我
- 库 jquery underscore zepto animate.css
我们调用他

### 渐进式 （渐进增强）
- vue全家桶 vuejs + vue-router + vuex + axios
- 通过组合 完成一个完整的框架

### MVC（backbone） 单向
- model数据
- view 视图
- controller 控制器

### MVVM(angular,vue) 双向的
- model数据
- view 视图
- viewModel 视图模型

### Object.defineProperty(es5)的没有替代方案
- 不支持ie8<=
```
Object.defineProperty(obj,"name",{
        configurable:true,//是否可以配置(删除)
        writable:false, //是否可重新赋值
        enumerable:false,//是否可枚举(遍历)
        value:1
    })


 let obj={};
 let temp={};
    Object.defineProperty(obj,'name',{
        get(){
            //取obj的name属性会触发
            return temp["name"];
        },
        set(val){
            //给obj赋值会触发set方法
            temp["name"]=val;//改变temp的结果
            input.value=val;//将值赋给输入框
        }
    });
    input.value=obj.name;//页面一加载时,会调用get方法
    input.addEventListener('input',function () {//等待输入框的变化
        obj.name=this.value;//当值变化时会调用set方法
    })
```

### vm
- vm => viewModel 数据最终都会被vm所代理。
- {{a}} 取值表达式，通过{{}}来进行取值,默认可以不写this，表达式 赋值运算 计算 三元表达式
尽量少写逻辑（computed）

### vue的指令(direvtive)
vue的"指令"directive 只是dom上的行间属性,vue给这类属性赋予一定的意义来实现特殊的功能,所有指令都以v-开头,value属性默认情况下会被vue忽略掉 selected checked都没有意义;

1. `v-model` (表单元素) 忽略掉value,checked,selected,将数据绑定的视图上，视图修改后会影响数据的变化;
v-model(如果checkbox,select多选是数组，提供一个value属性)(radio checkbox分组靠的是v-model),checked selected不存在
2. `v-text `取代{{}}
`v-cloak 可以修复闪烁(块)问题
   要给style  [v-cloak]{display:none}`
3. `v-once` 绑定一次，数据在变化不会导致视图刷新，写在不想刷新的标签上
4. `v-html` 将html字符串转化成html
5. `v-for` 循环(数组，对象，字符串，数字)

  ```
   <div v-for="value in/of 数组"></div>
   <div v-for="(value,index) in/of 数组"></div>
  ```
#### v-for
以前 :拼接字符串innerHTML
   `vue提供了一个指令 v-for 解决循环问题  更高效 会复用原有的解构`
  - 要循环谁就在谁身上加v-for属性
  - 默认value in 数组 /(value,index) in 数组

  ```
   <ul>
        <li v-for="(fruit,index) in fruits">{{index+1}}.{{fruit.name}}
   </li>
   </ul>
  ```

#### v-show
根据值ture/false来控制元素的display属性;
操作的是样式

#### v-if
操作的是dom
`v-if`和`v-else-if`链式调用
根据值的true/false,在切换时元素及它的数据绑定 / 组件被销毁并重建
template标签 是vue提供给我们的没有任何的实际意义，用来包裹元素用的，v-show不支持template,只有v-if使用.在结构中不显示
```
<template v-if="true">
        <div>123</div>
        <div>123</div>
        <div>123</div>
</template>
```
#### v-else
前一兄弟元素必须有 v-if 或 v-else-if。

#### v-bind
动态地绑定一个或多个特性
`<img :src="src" :width="w"/>`
在绑定class和style时候
第一种方式是对象 第二种方式是数组  比较常用
```
<div class="x" :class="{z:flag,y:true}">我很帅</div>
  <div class="x" :class="[class1,class2,{z:false}]">我很帅</div>

  <div :class="{true:'x',false:'y'}[true]">你不会用吧？</div>
  <div v-for="(a,index) in 10" :class="{x:index%2}">{{a}}</div>
```
### 事件v-on(@)
  绑定给dom元素，函数需要定义在methods中，不能和data里的内容重名，this指向的是实例，不能使用箭头函数，事件源对象如果不写括号，可以自动传入，否则只能手动传入$event
```
<div @事件名="fn"></div>
```
如果不传递参数 则不要写括号,如果写括号,则要手动传入$event属性
```
<div @mousedown="fn($event,1)">点我啊</div>
```
```
let vm = new Vue({//根实例
        el:'#app',
        methods:{
            //methods和data中的数据会全部放到vm上,而且名字不能冲突,冲突会报错,methods中的this指向的都是实例
//            fn:function () {alert(this)}
            fn(event,a){alert(this.a)}  //Es6写法
        },
        data:{//data里都是数据
            a:1
        }
    })
```

### 修饰符
修饰符 .number.lazy
- 按键修饰符 .enter.ctrl.keyCode
- 事件
    - @事件.stop,   阻止事件传播
    ```
    stopPropagation,cancelBubble=true;
    ```
    - @事件.capture,  触发捕获
    ```
    xxx.addEventListener('click',fn,true)
    ```
    - @事件.prevent,  阻止事件默认行为
    ```
     preventDefault,returnValue=false
    ```
    - @事件.once,  事件只执行一次
    ```
    on('click') off('click')
    ```
    - @事件.self,  判断事件源绑定事件
    ```
    e.srcElement&&e.target
    ```
## filters实例上可以用
```
{{'123' | my(1,2,3)}}
filters:{
    my(data,param1,param2,param3){

    }
}
```

## computed计算“属性” 不是方法
- 方法不会有缓存，computed会根据依赖(归vue管理的数据，可以响应式变化的)的属性进行缓存
- 两部分组成有get和set(不能只写set) 一般情况下 通过js赋值影响其他人或者表单元素设置值的时侯会调用set方法;
- 不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
```
let vm = new Vue({
        el:'#app',
        data:{a:''},
        computed:{//默认调用get方法,必须要有return;computed不支持异步
            msg(){
                if(this.a.length===0){
                    return '请输入'
                }
                if(this.a.length <3){
                    return '少了'
                }
                if(this.a.length >6){
                    return '多了'
                }
                return '';
            }
        }
    })
```

### watch
只有值变化的时候才会触发 支持异步了,其他情况我们更善于使用computed;
watch的属性名字要和观察的人的名字一致
```
 let vm = new Vue({
        el:'#app',
        data:{a:'',msg:''},
        /*watch:{
            a(newVal,oldVal){
                this.msg = '...........'
                setTimeout(()=>{
                    if(newVal.length<3){
                        return this.msg = '太少'
                    }
                    if(newVal.length>6){
                        return this.msg = '太多'
                    }
                    this.msg = '';
                },2000)
            }
        }*/
    });
```
### directive
自定义指令放在这个属性中
```
 let vm = new Vue({
        directives:{
            drag(el){
              el.onmousedown = function (e) {
                  var disx = e.pageX - el.offsetLeft;
                  var disy = e.pageY - el.offsetTop;
                  document.onmousemove = function (e) {
                      el.style.left = e.pageX - disx+'px' ;
                      el.style.top = e.pageY - disy+'px' ;
                  };
                  document.onmouseup = function () {
                      document.onmousemove = document.onmouseup = null
                  };
                  e.preventDefault();
              }
            },
            color(el,bindings){ //el指代的是button按钮
                el.style.background =bindings.value;
            }
        },
        el:'#app',
        data:{
          flag:'red'
        }
    })
```

### promise
表示请求数据成功后.用promise干点什么事
-  resolve代表的是转向成功态
- reject代表的是转向失败态   resolve和reject均是函数
- promise的实例就一个then方法,then方法中有两个参数
let p = new Promise((resolve,reject)=>{
    setTimeout(function () {
        let a = '蘑菇';
        reject(a);
    },2000)
});
p.then((data)=>{console.log(data)},(err)=>{console.log('err')});

回调函数 将后续的处理逻辑传入到当前要做的事，事情做好后调用此函数
- promise 解决回调问题的 promise三个状态
- 成功态 失败态 等待(pending)


### vue的基本结构
```
  <div id="app">

  </div>
<script>
   let vm=new Vue({
      el:'#app',
      created(){}, //在数据初始化后被使用 (钩子函数) **目前唯一函数**
      data:{},     //数据 (一些变量：用来存储值)
      methods:{},  //函数 (方法)
      computed:{}, //计算 (值并非设定,还是通过其他数据的计算得来)
      filters:{},  //过滤器  (可以对数据({{获取的数据}})进行加工
      格式:( {{a}} | 自定义过滤器名字(参数) ) ) 实例=》如下
      watch:{}     //监控数据变化(可以用于异步) 实例=》如下
      directives:(){}  //自定义指令(v-...)
</script>
```

```
<body>
<div id="app">
  <div>
      {{a.num*a.price    |     toFixed(2)}}</div>
  <!--   input         管道符      过滤方法   toFixed(2):保留小数点后两位有效数字 -->
</div>
<script src="..de_modulesue/distue.js"></script>
<script>
    let vm = new Vue({
        el:'#app',
        data:{
             a:{num:4,price:50.4},
        },
        filters:{
            toFixed(input,param){
              // {{    a           |       toFixed    (2)   }}
              //    input表示a    管道符     过滤方法    参数
                return '￥'+input.toFixed(param)
            }
        }
    })
</script>
</body>
```




