### Vue 

- 构建用户界面的渐进式框架**
- vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合

### 

###  指令

- 本质就是自定义属性
- Vue中指定都是以 v- 开头 

###  v-cloak

- 防止页面加载时出现闪烁问题

  ```html
   <style type="text/css">
    /* 
      1、通过属性选择器 选择到 带有属性 v-cloak的标签  让他隐藏
   */
    [v-cloak]{
      /* 元素隐藏    */
      display: none;
    }
    </style>
  <body>
    <div id="app">
      <!-- 2、 让带有插值 语法的   添加 v-cloak 属性 
      -->
      <div  v-cloak  >{{msg}}</div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
      var vm = new Vue({
        //  el   指定元素 id 是 app 的元素  
        el: '#app',
        //  data  里面存储的是数据
        data: {
          msg: 'Hello Vue'
        }
      });
  </script>
  </body>
  </html>
  ```
  

###  v-text

- v-text指令用于将数据填充到标签中，类似插值表达式，但是没有闪动问题
- 单向绑定，数据对象上的值改变，插值会发生变化；插值发生变化并不会影响数据对象的值

```html
<div id="app">
    <!--  
		注意:在指令中不要写插值语法  直接写对应的变量名称 
        在 v-text 中 赋值的时候不要在写 插值语法
		一般属性中不加 {{}}  直接写 对应 的数据名 
	-->
    <p v-text="msg"></p>
    <p>
        <!-- Vue  中只有在标签的 内容中 才用插值语法 -->
        {{msg}}
    </p>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });

</script>
```

###  v-html

- 用法和v-text 相似  但是他可以将HTML片段填充到标签中

- 可能有安全问题，xss攻击, 一般只在可信任内容上使用 `v-html`，**永不**用在用户提交的内容上

- 它与v-text区别在于v-text输出的是纯文本，浏览器不会对其再进行html解析，但v-html会将其当html标签解析后输出。

  

###  v-pre

- 显示原始信息跳过编译过程





### 数据响应式

数据绑定 单向的

### **v-once**

- 执行一次性的插值   没有响应式的功能，可以提高性能，当不需要再修改

```html
     <span v-once>{{ msg}}</span>    
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });
</script>
```



### 双向数据绑定

- 当数据发生变化的时候，视图也就发生变化
- 当视图发生变化的时候，数据也会跟着同步变化

#### v-model

- **v-model**是一个指令，限制在 `<input>、<select>、<textarea>、components`中使用

```html
 <div id="app">
      <div>{{msg}}</div>
      <div>
         <!--当输入框中内容改变的时候页面上的msg  会自动更新-->
        <input type="text" v-model='msg'>
      </div>
  </div>
```

### mvvm

- MVC 是后端的分层开发概念； MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel
- m   model  
  - 数据层   Vue  中 数据层 都放在 data 里面
- v   view     视图   
  - Vue  中  view      即 我们的HTML页面  
- vm   （view-model）     控制器     将数据和视图层建立联系      
  - 从事件到模型用的事事件监听(DOM Listeners)
  - 从模型到事件就是数据绑定（date Bin dings）
  - 

###   v-on

- 用来绑定事件的
-  形式如：v-on:click  缩写为 @click;



###  v-on事件函数中传入参数

```html

<body>
    <div id="app">
        <div>{{num}}</div>
        <div>
            <button v-on:click='handle1'>点击1</button>
        
            <button v-on:click='handle2(123, 456, $event)'>点击2</button>
        </div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        /*
        事件绑定函数名称，默认传递事件对象为第一个参数
        事件绑定函数调用，事件对象必须作为最后一个参数传递，并且是$event
        */
        var vm = new Vue({
            el: '#app',
            data: {
                num: 0
            },
            methods: {
                handle1: function(event) {
                    console.log(event.target.innerHTML)
                },
                handle2: function(p, p1, event) {
                    console.log(p, p1)
                    console.log(event.target.innerHTML)
                    this.num++;
                }
            }
        });
    </script>
```

###  事件修饰符

- 修饰符是由点开头的指令后缀来表示的

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联   即阻止冒泡也阻止默认事件 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，
用 v-on:click.prevent.self 会阻止所有的点击，
而 v-on:click.self.prevent 只会阻止对元素自身的点击。
```

### 按键修饰符

- 提交表单有时候可以按enter键，所以需要绑事件

```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">

<!-- -当点击enter 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">

<!--当点击enter或者space时  时调用 `vm.alertMe()`   -->
<input type="text" v-on:keyup.enter.space="alertMe" >

常用的按键修饰符
.enter =>    enter键
.tab => tab键
.delete (捕获“删除”和“退格”按键) =>  删除键
.esc => 取消键
.space =>  空格键
.up =>  上
.down =>  下
.left =>  左
.right =>  右

<script>
	var vm = new Vue({
        el:"#app",
        methods: {
              submit:function(){},
              alertMe:function(){},
        }
    })

</script>
```

### 自定义按键修饰符别名

- 在Vue中可以通过`config.keyCodes`自定义按键修饰符别名



## 属性绑定

### v-bind

v-bind 指令被用来响应地更新 HTML 属性

v-bind:href    可以缩写为    :href;

```html
<div id="app">
  <a v-bind:href="url">百度</a>
  <a :href="url">百度1</a>
  <button v-on:click='handle'>切换</button>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
  /*
    属性绑定
  */ 
  var vm = new Vue({
    el: '#app', 
    data: {
      url: 'http://www.baidu.com'
    },
    methods: {
      handle: function(){
        // 修改URL地址
        this.url = 'http://itcast.cn';
      }
    }
  });
</script>
```

```html
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc">

<!-- 缩写 -->
<img :src="imageSrc">
```

#### 绑定对象

- 注意：v-bind:class指令可以与普通的class特性共存

```html

<ul class="box" v-bind:class="{textColor:isColor, textSize:isSize}">
    <li>学习Vue</li>
    <li>学习Node</li>
    <li>学习React</li>
</ul>
  <div v-bind:style="{color:activeColor,fontSize:activeSize}">对象语法</div>

<sript>
var vm= new Vue({
    el:'.box',
    data:{
        isColor:true,
        isSize:true，
    	activeColor:"red",
        activeSize:"25px",
    }
})
</sript>
<style>

    .box{
        border:1px dashed #f0f;
    }
    .textColor{
        color:#f00;
        background-color:#eef;
    }
    .textSize{
        font-size:30px;
        font-weight:bold;
    }
</style>
```

####  绑定class

```html


这里的classA  对用data 中的  classA
这里的classB  对用data 中的  classB
<ul class="box" :class="[classA, classB]">
    <li>学习Vue</li>
    <li>学习Node</li>
    <li>学习React</li>
</ul>
<script>
var vm= new Vue({
    el:'.box',
    data:{
        classA:‘textColor‘,
        classB:‘textSize‘
    }
})
</script>
<style>
    .box{
        border:1px dashed #f0f;
    }
    .textColor{
        color:#f00;
        background-color:#eef;
    }
    .textSize{
        font-size:30px;
        font-weight:bold;
    }
</style>
```



### 分支结构

#### v-if 使用场景

- 1- 多个元素 通过条件判断展示或者隐藏某个元素。或者多个元素
- 2- 进行两个视图之间的切换

```html
<div id="app">
        <!--  判断是否加载，如果为真，就加载，否则不加载-->
        <span v-if="flag">
           如果flag为true则显示,false不显示!
        </span>
</div>

<script>
    var vm = new Vue({
        el:"#app",
        data:{
            flag:true
        }
    })
</script>

----------------------------------------------------------

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

<script>
    new Vue({
      el: '#app',
      data: {
        type: 'C'
      }
    })
</script>
```

### xun环结构

#### v-for

- 用于循环的数组里面的值可以是对象，也可以是普通元素  

```html
<ul id="example-1">
   <!-- 循环结构-遍历数组  
	item 数组里面的每一项  
	items  data中的数组-->
  <li v-for="item in items">
    {{ item.message }}
  </li> 

</ul>
<script>
 new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]，
   
  }
})
</script>
```

- 当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。






