## deep:true

对本方法来说，handler无法监听到新旧值的变化，只会监听到最新的值

```html
<div id="app">
    <h2>{{message.c}}</h2>
    <button @click="change()">改变</button>
</div>


<script src="../js/vue.js"></script>
<script>
const vue = new Vue({
  el: '#app',
  data: {
    message: {
      c:"对象的内部属性"
    }
  },
  watch:{
    message: {
      handler(ne,ol){
        console.log("新")
        console.log(ne.c)

        console.log("旧")
        console.log(ol.c)
      },
      deep:true
    }
  },
  methods:{
    change(){
      this.message.c="对象改变了"
    }
  }
})
```

![image-20210130153957422](C:%5CUsers%5CAdministrator%5CDesktop%5C%E5%AD%A6%E4%B9%A0%E6%96%87%E6%A1%A3%5Cmd%5C%E5%9B%BE%E7%89%87%5Cimage-20210130153957422.png)