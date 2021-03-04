

### 表单基本操作

- 获取单选框中的值

  - 通过v-model

  ```html
  
     <input type="radio" id="male" value="1" v-model='gender'>
     <label for="male">男</label>
  
     <input type="radio" id="female" value="2" v-model='gender'>
     <label for="female">女</label>
  
  <script>
      new Vue({
           data: {
               // 默认会让当前的 value 值为 2 的单选框选中
                  gender: 2,  
              },
      })
  
  </script>
  ```
  
- 获取复选框中的值

  - 通过v-model
  - 和获取单选框中的值一样 
  - 复选框 `checkbox` 这种的组合时   data 中的 hobby 我们要定义成数组 否则无法实现多选

  ```html
  <div>
     <span>爱好：</span>
     <input type="checkbox" id="ball" value="1" v-model='hobby'>
     <label for="ball">篮球</label>
     <input type="checkbox" id="sing" value="2" v-model='hobby'>
     <label for="sing">唱歌</label>
     <input type="checkbox" id="code" value="3" v-model='hobby'>
     <label for="code">写代码</label>
   </div>
  <script>
      new Vue({
           data: {
                  // 默认会让当前的 value 值为 2 和 3 的复选框选中
                  hobby: ['2', '3'],
              },
      })
  </script>
  ```
  
- 获取下拉框和文本框中的值

  - 通过v-model

  ```html
     <div>
        <span>职业：</span>
  
         <!-- multiple  多选 -->
        <select v-model='occupation' multiple>
            <option value="0">请选择职业...</option>
            <option value="1">教师</option>
            <option value="2">软件工程师</option>
            <option value="3">律师</option>
        </select>
           <!-- textarea 是 一个双标签   不需要绑定value 属性的  -->
          <textarea v-model='desc'></textarea>
    </div>
  <script>
      new Vue({
           data: {
                  // 默认会让当前的 value 值为 2 和 3 的下拉框选中
                   occupation: ['2', '3'],
               	 desc: 'nihao'
              },
      })
  </script>
  ```

### 表单修饰符

- .number  转换为数值

- .trim  自动过滤用户输入的首尾空白字符

- .lazy   将input事件切换成change事件

- 在失去焦点 或者 按下回车键时才更新
  
  ```html
  <!-- 自动将用户的输入值转为数值类型 -->
  <input v-model.number="age" type="number">
  
  <!--自动过滤用户输入的首尾空白字符   -->
  <input v-model.trim="msg">
  
  <!-- 在“change”时而非“input”时更新 -->
  <input v-model.lazy="msg" >
  ```

###  自定义指令

- 内置指令不能满足我们特殊的需求
- Vue允许我们自定义指令

#### Vue.directive  注册全局指令

```html

<input type="text" v-focus>
<script>
Vue.directive('focus', {
  	// 当绑定元素插入到 DOM 中。 其中 el为dom元素
  	inserted: function (el) {
    		// 聚焦元素
    		el.focus();
 	}
});
new Vue({
　　el:'#app'
});
</script>
```

#### Vue.directive  注册全局指令 带参数

```html
  <input type="text" v-color='msg'>
 <script type="text/javascript">
    Vue.directive('color', {
      bind: function(el, binding){
        el.style.backgroundColor = binding.value.color;
      }
    });
    var vm = new Vue({
      el: '#app',
      data: {
        msg: {
          color: 'blue'
        }
      }
    });
  </script>
```



###  计算属性   computed

- 模板中放入太多的逻辑会让模板过重且难以维护  使用计算属性可以让模板更加的简洁
- **计算属性是基于它们的响应式依赖进行缓存的**
- computed比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化

```html
 <div id="app">
    <div>{{reverseString}}</div>
    <div>{{reverseString}}</div>
    <div>{{reverseMessage()}}</div>
    <div>{{reverseMessage()}}</div>
  </div>
  <script type="text/javascript">
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Nihao',
        num: 100
      },
      methods: {
        reverseMessage: function(){
          console.log('methods')
          return this.msg.split('').reverse().join('');
        }
      },
      computed: {
        reverseString: function(){
          console.log('computed')
          var total = 0;  
          for(var i=0;i<=this.num;i++){
            total += i;
          }，    
          return total;
        }
      }
    });
  </script>
```

###  侦听器   watch

- 使用watch来响应数据的变化
- 一般用于异步或者开销较大的操作
- watch 中的属性 一定是data 中 已经存在的数据 
- **当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，只有data中的数据才能够监听到变化，此时就需要deep属性对对象进行深度监听**

```html
 <div id="app">
        <div>
            <span>名：</span>
            <span>
        <input type="text" v-model='firstName'>
      </span>
        </div>
        <div>
            <span>姓：</span>
            <span>
        <input type="text" v-model='lastName'>
      </span>
        </div>
        <div>{{fullName}}</div>
    </div>

  <script type="text/javascript">
        
        var vm = new Vue({
            el: '#app',
            data: {
                firstName: 'Jim',
                lastName: 'Green',       
            },
            
            watch: {        
                firstName: function(val) {
                    this.fullName = val + ' ' + this.lastName;
                },
                lastName: function(val) {
                    this.fullName = this.firstName + ' ' + val;
                }
            }
        });
    </script>
```









### 生命周期

- 事物从出生到死亡的过程
- Vue实例从创建 到销毁的过程 ，

### 钩子函数



| beforeCreate  | 在实例初始化之后，数据观测和事件配置之前被调用 此时data 和 methods 以及页面的DOM结构都没有初始化   什么都做不了 |
| ------------- | ------------------------------------------------------------ |
| created       | 在实例创建完成后被立即调用此时data 和 methods已经可以使用  但是页面还没有渲染出来 |
| beforeMount   | 在挂载开始之前被调用   此时页面上还看不到真实数据 只是一个模板页面而已 |
| mounted       | el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子。  数据已经真实渲染到页面上  在这个钩子函数里面我们可以使用一些第三方的插件 |
| beforeUpdate  | 数据更新时调用，发生在虚拟DOM打补丁之前。   页面上数据还是旧的 |
| updated       | 由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。 页面上数据已经替换成最新的 |
| beforeDestroy | 实例销毁之前调用                                             |
| destroyed     | 实例销毁后调用                                               |

### 数组变异方法

- 变异数组方法即保持数组方法原有功能不变的前提下对其进行功能拓展

| `push()`    | 往数组最后面添加一个元素，成功返回当前数组的长度             |
| ----------- | ------------------------------------------------------------ |
| `pop()`     | 删除数组的最后一个元素，成功返回删除元素的值                 |
| `shift()`   | 删除数组的第一个元素，成功返回删除元素的值                   |
| `unshift()` | 往数组最前面添加一个元素，成功返回当前数组的长度             |
| `splice()`  | 有三个参数，第一个是想要删除的元素的下标（必选），第二个是想要删除的个数（必选），第三个是删除 后想要在原位置替换的值 |
| `sort()`    | sort()  使数组按照字符编码默认从小到大排序,成功返回排序后的数组 |
| `reverse()` | reverse()  将数组倒序，成功返回倒序后的数组                  |

### 替换数组



### 动态数组响应式数据



####  

























































