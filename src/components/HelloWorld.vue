<template>
<div>
  <header>
    <h2>午茶時光</h2>
    <p>
      我想要點...
      <input type="text">
    </p>
  </header>
  <section>
    <ul>
      <li @click="sortBy('all')">
        <a href="javascript:void(0)">所有訂單 ({{ totalSum }})</a>
      </li>
      <li @click="sortBy('nodo')">
        <a href="javascript:void(0)">未完成 ({{ nodoSum }})</a>
      </li>
      <li @click="sortBy('done')">
        <a href="javascript:void(0)">已完成 ({{ doneSum }})</a>
      </li>
    </ul>
    <ul>
      <li v-for="(item, index) in list">
        <template>
          <input type="text" v-input-focus>
        </template>
        <template>
          <div>
            <input type="checkout" />
            <label for="">{{ item.item }}</label>
          </div>
          <div>
            <button>修改</button>
            <button>結單</button>
          </div>
        </template>
      </li>
    </ul>
  </section>
  </div>
</template>

<script>
export default {
  name: 'todoList',
  data() {
    return {
      sort: 'all',
      orderData: [{
        item: '綠茶',
        done: true
      }, {
        item: '紅茶',
        done: true
      }, {
        item: '綠茶',
        done: false
      }]
    }
  },
  computed: {
    list: function () {
      let type = this.sort;
      switch (type) {
        case 'all':
          return this.orderData;
        case 'nodo':
          return this.orderData.filter(v => {
            return !v.done;
          });
        case 'done':
          return this.orderData.filter(v => {
            return v.done;
          });
      }
    },
    totalSum: function () {
      return this.orderData.length;
    },
    nodoSum: function () {
      let nodo = this.orderData.filter(v => {
        return !v.done;
      });
      return nodo.length;
    },
    doneSum: function () {
      let done = this.orderData.filter(v => {
        return v.done;
      });
      return done.length;
    }
  },
  methods: {
    sortBy (type) {
      this.sort = type;
    }
  },
  directives :{
      inputFocus :{ //自訂標籤名稱
          inserted: function(el){
              el.focus();
          }
      }
  }
}
</script>
