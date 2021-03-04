

### 组件

- 组件 (Component) 是 Vue.js 最强大的功能之一
- 组件可以扩展 HTML 元素，封装可重用的代

### 组件注册

#### 全局注册

- Vue.component('组件名称', { })     第1个参数是标签名称，第2个参数是一个选项对象
- **全局组件**注册后，任何**vue实例**都可以用

##### 组件基础用

```html
<div id="example">
  <!-- 2、 组件使用 组件名称 是以HTML标签的形式使用  -->  
  <my-component></my-component>
</div>
<script>
    //   注册组件 
    // 1、 my-component 就是组件中自定义的标签名
	Vue.component('my-component', {
      template: '<div>A custom component!</div>'
    })

    // 创建根实例
    new Vue({
      el: '#example'
    })

</script>
```

##### 组件注意事项

- 组件参数的data值必须是函数同时这个函数要求返回一个对象 
- 组件模板必须是单个根元素
- 组件模板的内容可以是模板字符串



#### 局部注册

- 只能在当前注册它的vue实例中使用





### Vue组件之间传值

#### 父组件向子组件传值

- 父组件发送的形式是以属性的形式绑定值到子组件身上。
- 然后子组件用属性props接收

```html
  <div id="app">
    <div>{{pmsg}}</div>
    <menu-item title='来自父组件的值'></menu-item>
    <menu-item :title='ptitle' content='hello'></menu-item>
  </div>
  <script type="text/javascript">
    Vue.component('menu-item', { 
      props: ['title', 'content'],
      data: function() {
        return {
          msg: '子组件本身的数据'
        }
      },
      template: '<div>{{msg + "----" + title + "-----" + content}}</div>'
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        ptitle: '动态绑定属性'
      }
    });
  </script>
```

#### 子组件向父组件传值

- 子组件用`$emit()`触发事件
- `$emit()`  第一个参数为 自定义的事件名称     第二个参数为需要传递的数据
- 父组件用v-on 监听子组件的事件

```html

 <div id="app">
    <div :style='{fontSize: fontSize + "px"}'>{{pmsg}}</div>
    <menu-item :parr='parr' @enlarge-text='handle($event)'></menu-item>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    
    
    Vue.component('menu-item', {
      props: ['parr'],
      template: `
        <div>
          <ul>
            <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
          </ul>
			
          <button @click='$emit("enlarge-text", 5)'>扩大父组件中字体大小</button>
          <button @click='$emit("enlarge-text", 10)'>扩大父组件中字体大小</button>
        </div>
      `
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        parr: ['apple','orange','banana'],
        fontSize: 10
      },
      methods: {
        handle: function(val){
          
          this.fontSize += val;
        }
      }
    });
  </script>

```

#### 兄弟之间的传递

- 兄弟之间传递数据需要借助于事件中心，通过事件中心传递数据   
- 传递数据方，通过一个事件触发hub.$emit(方法名，传递的数据)
- 接收数据方，通过mounted(){} 钩子中  触发hub.$on()方法名
- 销毁事件 通过hub.$off()方法名销毁之后无法进行传递数据

```html
 <div id="app">
    <div>父组件</div>
    <div>
      <button @click='handle'>销毁事件</button>
    </div>
    <test-tom></test-tom>
    <test-jerry></test-jerry>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">

    var hub = new Vue();

    Vue.component('test-tom', {
      data: function(){
        return {
          num: 0
        }
      },
      template: `
        <div>
          <div>TOM:{{num}}</div>
          <div>
            <button @click='handle'>点击</button>
          </div>
        </div>
      `,
      methods: {
        handle: function(){
          hub.$emit('jerry-event', 2);
        }
      },
      mounted: function() {
   
        hub.$on('tom-event', (val) => {
          this.num += val;
        });
      }
    });
    Vue.component('test-jerry', {
      data: function(){
        return {
          num: 0
        }
      },
      template: `
        <div>
          <div>JERRY:{{num}}</div>
          <div>
            <button @click='handle'>点击</button>
          </div>
        </div>
      `,
      methods: {
        handle: function(){
         
          hub.$emit('tom-event', 1);
        }
      },
      mounted: function() {
       
        hub.$on('jerry-event', (val) => {
          this.num += val;
        });
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        
      },
      methods: {
        handle: function(){
          hub.$off('tom-event');
          hub.$off('jerry-event');
        }
      }
    });
  </script>

```



