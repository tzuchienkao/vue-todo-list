<template>
  <section>
    <ul class="flex-box tabs">
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
    <ul class="order-list">
      <li class="flex-box flex-h-align" v-for="(item, index) in list" :key="'item-' + index">
          <template v-if="item === editItem">
            <input type="text" v-model="item.item" @keydown.enter="doneEdit(item)" @keydown.esc="cancelEdit(item)" @blur="cancelEdit(item)" v-input-focus>
          </template>
          <template v-else>
            <div class="order-item">
              <input type="checkbox" :id="'item-' + index" v-model="item.done" />
              <label :for="'item-' + index" @dblclick="doEdit(item)">
                {{ item.item }}
              </label>
            </div>
            <div class="btn-group">
              <button @click="doEdit(item)">修改</button>
              <button @click="removeOrder(item)">結單</button>
            </div>
          </template>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: 'list',
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
      }],
      isEdit: false,
      editItem: null
    }
  },
  props: ['newItem'],
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
    newOrder: function () {
      this.$nextTick(() => {
      });
      return this.newItem;
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
  watch: {
    newOrder (val) {
      this.orderData.push({ item: val, done: false });
    }
  },
  methods: {
    sortBy (type) {
      this.sort = type;
    },
    doEdit (item) {
      this.editItem = item;
      this.beforeEditCache = item.item;
    },
    cancelEdit (item) {
      if (!this.isEdit) {
        this.editItem = null;
        item.item = this.beforeEditCache;
      }
      this.isEdit = false;
    },
    doneEdit (item) {
      if(!this.editItem) return;
      this.editItem = null;
      item.item = item.item.trim();
      if(!item.item){
          this.removeOrder(item)
      }
      this.isEdit = true;
    },
    removeOrder (item) {
      let delItem = this.orderData.indexOf(item);
      this.orderData.splice(delItem, 1);
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
