```html
<div v-for="item in list">
  <div>{{item.dataT}}</div>
  <a-table :columns="columns" :dataSource="data" bordered>
    <template slot="name" slot-scope="text">
      <a>{{text}}</a>
    </template>
  </a-table>
</div>
data () {
      return {
        list:[
          {name:'2222',age:22,dataT:'12345'},
          {name:'2222',age:23},
          {name:'2222',age:34},
          {name:'2222',age:33},
          {name:'2222',age:31},
        ]
	}
}
```

