<template>
  <div id="app" class="container">
    <h1 class="mt-4 mb-2 border-bottom">Calander</h1>
    <p>Selected Date: {{ c1selected | date }}</p>
    <Calander @onSelect="changeC1"></Calander>
  </div>
</template>

<script>
import Calander from './components/Calander.vue'

export default {
  name: 'app',
  components: {
    Calander
  },
  data: function(){
    return {
      c1selected: null,
    }
  },
  filters: {
    date: function(date) {
      if (!date) {
        return "NaN"
      }
      return date.getFullYear() + "-" + (date.getMonth() + 1) + "-" + date.getDate();
    }
  },
  methods: {
    changeC1: function(date) {
      this.c1selected = date;
    },
  },
}
</script>

<style lang="less">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.calander {
  margin: 0 auto;
  padding: 30px;
}
</style>
