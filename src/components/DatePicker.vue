<template>
    <div class="datepicker">
        <div class="input" @click="open = !open">
            <i class="fa fa-calendar icon"></i>
            <input :value="datestr" ref="input" @change="updateDate($event)" />
        </div>
        <Calendar v-show="open" ref="calendar" class="animated fadeInDown" @onSelect="onSelect"></Calendar>
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
            separator: "-",
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
            return ("000" + year).slice(-4) + this.separator + ("0" + month).slice(-2) + this.separator + ("0" + date).slice(-2);
        },
    },
    methods: {
        onSelect: function(date) {
            this.date = date;
        },
        validate: function(str) {
            return str.match('[0-9]{4}-[0-9]{2}-[0-9]{2}');
        },
        updateDate: function(e) {
            if (!this.validate(e.target.value)) {
                console.error('invalid date format');
                e.target.value = this.datestr;
                return;
            }
            this.date = new Date(e.target.value);
            this.$refs.calendar.date(this.date);
        },
    },
    mounted: function() {
        this.$refs.calendar.date(this.date);
    }
}
</script>

<style lang="less" scoped>
@gray: #eeeeee;

.datepicker {
    text-align: left;
    display: inline-block;
    
    .input {
        position: relative;
        cursor: pointer;

        .icon {
            position: absolute;
            top: 11px;
            left: 11px;
        }

        input {
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
        animation-duration: 0.2s;
    }
}

</style>
