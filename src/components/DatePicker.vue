<template>
    <div class="datepicker">
        <div class="input">
            <i class="fa fa-calendar icon"></i>
            <input @click="open = !open" :value="datestr" />
        </div>
        <Calander v-show="open" ref="calander" @onSelect="onSelect"></Calander>
    </div>
</template>

<script>
import Calendar from './Calendar.vue'

export default {
    name: "DatePicker",
    components: {
        Calendar,
    },
    data: function(){
        return {
            open: false,
            date: new Date(),
        }
    },
    computed: {
        datestr: function(){
            if (!this.date) {
                return "";
            }
            var year = this.date.getFullYear();
            var month = this.date.getMonth() + 1;
            var date = this.date.getDate();
            return ("000" + year).slice(-4) + "-" + ("0" + month).slice(-2) + "-" + ("0" + date).slice(-2);
        },
    },
    methods: {
        onSelect: function(date) {
            this.date = date;
        },
    },
}
</script>

<style lang="less" scoped>
@import '~font-awesome/css/font-awesome.min.css';

@gray: #eeeeee;

.datepicker {
    text-align: left;
    display: inline-block;
    
    .input {
        position: relative;

        .icon {
            position: absolute;
            top: 11px;
            left: 11px;
        }

        input {
            cursor: pointer;
            width: 130px;
            min-height: 35px;
            border: 1px solid @gray;
            border-radius: 5px;
            padding: 7px 12px;
            box-shadow: 1px 1px 5px @gray;
            text-align: right;
        }
    }

    .calendar {
        position: absolute;
    }
}

</style>
