<template>
  <section class="ui-table-layout tableWrap rel inline-block pd10">
    <table class="autoWidth" :class="{'table-scroll': hasScrollBar}" cellpadding="0" cellspacing="0">
      <thead ref='thd'>
        <tr>
          <th :class="{shortWidth: index===0 && hasCheck, floatBar: index === tableTitles.length - 1 && hasScrollBar}" v-for="(item, index) in tableTitles">
            {{ item }}
            <section class="checkbox" v-if="index === 0 && hasCheck && dataList.length > 1">
                <input @click="allCheck" :id="cId" type="checkbox" v-model="checkVal" :value="cId" />
                <label :for="cId"></label>
            </section>
          </th>
        </tr>
      </thead>
      <tbody :ref='bodyId'>
        <tr v-for="(item, i) in dataList">
          <td v-if="hasCheck" class="shortWidth">
              <section class="checkbox">
                <input :id="'slOne_' + item.id" type="checkbox" :value="item.id" v-model="selectedItems"/>
                <label :for="'slOne_' + item.id"></label>
              </section>
          </td>
          <slot :datas='item'></slot>
        </tr>
        <tr v-if="dataList.length === 0"><td :colspan="tableTitles.length" >----{{loadTxt}}----</td></tr>
      </tbody>
    </table>
  </section>
</template>
<script>
  export default {
    name: 'Vue-table',
    props: ['titles', 'datas', 'isCheck', 'loadTxt', 'checkId', 'tbodyId'],
    data () {
      return {
        hasCheck: false,
        hasScroll: false,
        dataList: [],
        tableTitles: [],
        selectedItems: [],
        cId: 'quan',
        checkVal: [],
        hasScrollBar: false,
        bodyId: 'tbodyId'
      }
    },
    created () { // 视图被创建
      this.setOptions();
    },
    methods: {
      setOptions () {
        this.dataList = this.datas || [];
        this.hasCheck = this.isCheck || false;
        this.tableTitles = this.titles || [];
        this.cId = this.checkId || 'quan';
        this.bodyId = this.tbodyId || 'tbodyId';
      },
      allCheck (e) {
        this.selectedItems = [];
        if (e.target.checked) {
          let self = this;
          self.dataList.forEach(function (er) {
            self.selectedItems.push(er.id);
          });
        }
      },
      changeTableHeight () {
        let self = this;
        let val = self.wHeight;
        this.$nextTick(() => {
          let tmp = val - 280;
          if (self.$refs[self.bodyId].scrollHeight > tmp) {
            self.$refs[self.bodyId].style.height = tmp + 'px';
            self.hasScrollBar = true;
          } else {
            self.$refs[self.bodyId].style.height = 'auto';
            self.hasScrollBar = false;
          }
        });
      }
    },
    watch: {
      datas () {
        this.dataList = this.datas || [];
        this.selectedItems = [];
        this.checkVal = [];
        this.changeTableHeight();
      },
      titles () {
        this.tableTitles = this.datas || [];
      },
      isCheck (val) {
        this.hasCheck = val || false;
      },
      selectedItems (val) {
        if (val.length === this.dataList.length && val.length > 1) {
          this.checkVal[0] = this.cId;
        } else {
          this.checkVal = [];
        }
        this.$emit('onChange', val);
      },
      wHeight () {
        this.changeTableHeight();
      }
    },
    computed: {
      wHeight () {
        return window.innerHeight;
      }
    }
  }
</script>
<style lang="scss" scoped="scoped">
section{
  display: block;
}
.inline-block{
  display: inline-block;
}
.pd10{
  padding: 10px;
}
.tableWrap{
  width: 100%;
  background: rgba(253,253,253,.95);
  table{
    width: 100%;
    min-width: 1000px;
    table-layout: fixed;
  }
  table.autoWidth{
    min-width: auto;
  }
  tbody tr{
      overflow: hidden;
  }
  thead.hide{
    display: none;
  }
  th{
      border: 1px solid #dfe6ec;
      border-left: none;
      background-color: #FFFFFF;
      text-align: center;
      padding: 10px 4px;
  }
  th:nth-child(1), td:nth-child(1){
      border-left: 1px solid #dfe6ec;
  }
  th.shortWidth, td.shortWidth{
    width: 40px;
  }
  td{
      border-bottom: 1px solid #dfe6ec;
      border-right: 1px solid #dfe6ec;
      text-align: center;
      padding: 10px 4px;
      word-break: break-all;
  }
  table.table-scroll{
    tbody{
      display: block;
      overflow-y: auto;
    }
    thead, tbody tr{
      display: table;
      width: 100%;
      table-layout: fixed;
    }
    thead{
      width: calc(100% - 17px);
    }
    .floatBar{
      position: relative;
      &:after{
        width: 17px;
        border-right: 1px solid #dfe6ec;
        border-bottom: 1px solid #dfe6ec;
        border-top: 1px solid #dfe6ec;
        padding: 11px 0 12px 0;
        right: -17px;
        top: -1px;
        background-color: #fff;
        content: '\00a0';
        position: absolute;
    }
    }
  }
}
.checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
    padding-left: 20px;
    label{
      min-height: 20px;
      padding-left: 0;
      margin-bottom: 0;
      font-weight: normal;
      cursor: pointer;
      display: inline-block;
      vertical-align: middle;
      position: relative;
      padding-left: 2px;
    }
    input[type="checkbox"] {
      position: absolute;
      margin-top: 4px \9;
      margin-left: -20px;
      opacity: 0;
      z-index: 1;
      cursor: pointer;
    }
}
.checkbox + .checkbox {
  margin-top: 0;
}
input[type="checkbox"][disabled],
input[type="checkbox"].disabled{
  cursor: not-allowed;
}
.checkbox label::before {
  content: "";
  display: inline-block;
  position: absolute;
  width: 17px;
  height: 17px;
  left: 0;
  margin-left: -20px;
  border: 1px solid #cccccc;
  border-radius: 3px;
  background-color: #fff;
  -webkit-transition: border 0.15s ease-in-out, color 0.15s ease-in-out;
  -o-transition: border 0.15s ease-in-out, color 0.15s ease-in-out;
  transition: border 0.15s ease-in-out, color 0.15s ease-in-out;
}
.checkbox label::after {
  display: inline-block;
  position: absolute;
  width: 16px;
  height: 16px;
  left: 0;
  top: -3px;
  margin-left: -22px;
  padding-left: 3px;
  padding-top: 1px;
  font-size: 18px;
  color: #FFFFFF;
}
.checkbox input[type="checkbox"]:checked + label::before{
    background-color: #F04055;
    border: 1px solid #F04055;
}
.checkbox input[type="checkbox"]:focus + label::before {
  outline: thin dotted;
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
.checkbox input[type="checkbox"]:checked + label::after{
  font-family: 'iconfont';
  content: "\e633";
}
.checkbox input[type="checkbox"]:disabled + label{
  opacity: 0.65;
}
.checkbox input[type="checkbox"]:disabled + label::before{
  background-color: #eeeeee;
  cursor: not-allowed;
  border-color: #cccccc;
}
.checkbox input[type="checkbox"]:disabled + label::after{
  cursor: not-allowed;
  color: #333;
}
.checkbox.checkbox-circle label::before {
  border-radius: 50%;
}
input[type="checkbox"].styled:checked + label:after{
  font-family: 'iconfont';
  content: "\e633";
  color: #fff;
}
input[type="checkbox"] .styled:checked + label::before{
  color: #fff;
}
</style>
