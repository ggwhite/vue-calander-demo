<template>
  <div id="app" class="container">
    <h1 class="mt-4 mb-2 border-bottom">Calander</h1>
    <p>Selected Date: {{ c1selected | date }}</p>
    <div class="calander-demo container">
      <Calander @onSelect="changeC1"></Calander>
    </div>

    <h1 class="mt-4 mb-2 border-bottom">DatePicker</h1>
    <div class="datepicker-demo container">
      <DatePicker></DatePicker>
    </div>
  </div>
</template>

<script>
import Calander from './components/Calander.vue'
import DatePicker from './components/DatePicker.vue'

export default {
  name: 'app',
  components: {
    Calander,
    DatePicker
  },
  data: function(){
    return {
      c1selected: null,
    }
  },
  filters: {
    date: function(date) {
      if (!date) {
        return "";
      }
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var date = date.getDate();
      return ("000" + year).slice(-4) + "-" + ("0" + month).slice(-2) + "-" + ("0" + date).slice(-2);
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

.calander-demo {
  width: 600px;
  padding: 30px;
  .calander {
    display: inline-block;
  }
}

.datepicker-demo {
  width: 600px;
  padding: 30px;
}


</style>
