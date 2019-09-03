<template>
  <header>
    <h2>午茶時光</h2>
    <p>
      我想要點...
      <input type="text" v-model="newOrder" @keydown.enter="addOrder()">
    </p>
  </header>
</template>

<script>
export default {
  name: 'top',
  data() {
    return {
      newOrder: ''
    }
  },
  methods: {
    addOrder () {
      let item = this.newOrder && this.newOrder.trim();
      if(!item) return;
      this.$emit('getItem', this.newOrder);
      this.newOrder = '';
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
      // if(!item.item){
      //     this.removeData(item)
      // }
      this.isEdit = true;
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
