<template>
  <div id="app" class="container">
    <h1 class="mt-4 mb-2 border-bottom">Calendar</h1>
    <p>Selected Date: {{ c1selected | date }}</p>
    <div class="calendar-demo container">
      <Calendar @onSelect="changeC1"></Calendar>
    </div>

    <h1 class="mt-4 mb-2 border-bottom">DatePicker</h1>
    <div class="datepicker-demo container">
      <DatePicker></DatePicker>
    </div>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue'
import DatePicker from './components/DatePicker.vue'

export default {
  name: 'app',
  components: {
    Calendar,
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

.calendar-demo {
  width: 600px;
  padding: 30px;
  .calendar {
    display: inline-block;
  }
  @media (max-width: 480px) {
    width: 100%;
    padding: 5px;
    text-align: left;
  }
}

.datepicker-demo {
  width: 600px;
  padding: 30px;
  @media (max-width: 480px) {
    width: 100%;
    padding: 5px;
    text-align: left;
  }
  .calendar {
    @media (max-width: 480px) {
      width: calc(100% - 40px);
    }
  }
}


</style>
