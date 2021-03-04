
Vuex是实现组件全局状态（数据）管理的一种机制，可以方便的实现组件之间的数据共享

主要就是数据共享

组件之间共享数据的方式

- 父向子：v-bind
- 子向父：v-on
- 兄弟组件之间：EventBus

使用Vuex管理数据的好处：

- 能够在vuex中集中管理共享的数据，便于开发和后期进行维护
- 能够高效的实现组件之间的数据共享，提高开发效率
- 存储在vuex中的数据是响应式的，当数据发生改变时，页面中的数据也会同步更新

在组件中共享的，才有必要存储到vuex中，私有的，依旧存在自身的data中即可




State
    State提供唯一的公共数据源，所有共享的数据都要统一放到Store中的State中存储
    

    在组件中访问State的方式：
    1).this.$store.state.全局数据名称  如：this.$store.state.count
    2).先按需导入mapState函数： import { mapState } from 'vuex'
    然后数据映射为计算属性： computed:{ ...mapState(['全局数据名称']) }

Mutation
Mutation用于修改变更$store中的数据
使用方式：
打开store.js文件，在mutations中添加代码如下

```
mutations: {
    add(state,step){
      //第一个形参永远都是state也就是$state对象
      //第二个形参是调用add时传递的参数
      state.count+=step;
    }
  }
```
然后在Addition.vue中给按钮添加事件代码如下：
```
<button @click="Add">+1</button>

methods:{
  Add(){
    //使用commit函数调用mutations中的对应函数，
    //第一个参数就是我们要调用的mutations中的函数名
    //第二个参数就是传递给add函数的参数
    this.$store.commit('add',10)
  }
}
```

使用mutations的第二种方式：


```
import { mapState,mapMutations } from 'vuex'
export default {
  data() {
    return {}
  },
  methods:{
      ...mapMutations(['sub']),
      Sub(){       
          this.sub(10);
      }
  },
  computed:{
      ...mapState(['count'])
      
  }
}
```



Action
在mutations中不能编写异步的代码，会导致vue调试器的显示出错。

```
actions: {
  addAsync(context,step){
    setTimeout(()=>{
      context.commit('add',step);
    },2000)
  }
}
```

```
<button @click="AddAsync">...+1</button>

methods:{
  AddAsync(){
    this.$store.dispatch('addAsync',5)
  }
}
```




Getter
Store中的数据进行加工处理形成新的数据
只会包装Store中保存的数据，并不会修改Store中保存的数据
：
```
export default new Vuex.Store({
  .......
  getters:{
    //添加了一个showNum的属性
    showNum : state =>{
      return '最新的count值为：'+state.count;
    }
  }
})
```
然后打开Addition.vue中，添加插值表达式使用getters
<h3>{{$store.getters.showNum}}</h3>















