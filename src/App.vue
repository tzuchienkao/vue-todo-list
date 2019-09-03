<template>
  <div id="app">
    <orderTop @getItem="getItem" />
    <orderList :newItem="newItem" />
  </div>
</template>

<script>
import orderTop from './components/orderTop.vue'
import orderList from './components/orderList.vue'

export default {
  name: 'app',
  components: {
    orderTop,
    orderList
  },
  data () {
    return {
      newItem: ''
    }
  },
  methods: {
    getItem(val) {
      console.log(val);
      this.newItem = val;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
