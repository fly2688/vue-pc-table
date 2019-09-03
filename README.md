# vue-pc-table

> table for pc based Vue2.x(当table超出最大高度时，tbody会自动出现滚动条，thead不动)

## Setup

``` bash
npm install vue-pc-table
```

## Detail
### html

```html
  <vueTable :titles='tableTitles' :datas='dataList' :isCheck='true' :isLoad='isLoad' :maxHeight='230'>
    <template slot-scope="tableItem">
      <td>{{tableItem.datas.userName | isEmpty}}</td>
      <td>{{tableItem.datas.adr | isEmpty}}</td>
      <td>{{tableItem.datas.content | isEmpty}}</td>
      <td>
        <button type="button" class="btn-danger btn-mg">删除</button>
      </td>
    </template>
  </vueTable>
```
### js
```js
import vueTable from 'vue-pc-table'
export default {
  name: 'app',
  data () {
    return {
      isLoad: false,
      tableTitles: ['', '名字', '地址', '内容', '操作'],
      dataList: [
        {userName: '叶子', adr: '青浦区', content: 'asad'},
        {userName: '阿斯达', adr: '哈萨克斯', content: 'd'},
        {userName: '阿萨德', adr: '阿联酋地区', content: '----'},
        {userName: '薇姿', adr: '威尔士地区', content: 'as'},
        {userName: '萨德年少', adr: '扬州', content: 'aers'}]
    }
  },
  components: {
    vueTable
  }
}
```
![image](https://github.com/fly2688/vue-pc-table/blob/master/src/xiaoguotu.jpg)
## Api

| Options         | Type     | Description                 | Default | necessary |
|-----------------|:--------:|:---------------------------:|:--------:|:--------:|
| titles | array | thead内容 | [] | 是 |
| datas | array | tbody数据 | [] | 否 |
| isCheck | boolean | 表格是否拥有checkBox多选功能 | false | 否 |
| checkId | string | checkBox全选的id（避免同一页面有重复id，故可自行设置此id） | quan | 否 |
| bodyId | string | tbody的id（用于获取表格高度，以优化页面） | tbodyId | 否（有多个table时必写） |
| maxHeight | Number | 用于设置tbody的最大高度，数据过多时自动出现滚动 | 页面的70%高度 | 否 |
