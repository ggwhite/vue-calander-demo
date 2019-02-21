<template>
    <div class="calendar">
        <div class="controller row">
            <div class="col-2" @click="last"><i class="fa fa-angle-left"></i></div>
            <div class="col-8">
                <span v-show="mode == 'date'" @click="mode='month'">{{ current | month }} {{ current | year }}</span>
                <span v-show="mode == 'month'" @click="mode='year'">{{ current | year }}</span>
                <span v-show="mode == 'year'">{{ current | yearRange }}</span>
            </div>
            <div class="col-2" @click="next"><i class="fa fa-angle-right"></i></div>
        </div>
        <div class="datepicker" v-show="mode == 'date'">
            <div class="week-day" v-for="day in weeks" :key="day">{{ day }}</div>
            <div v-for="date in dates" :key="date.getTime()">
                <span :class="{ gray : !isSameMonth(date, current), red: isSameDate(date, today), selected: isSameDate(date, selected) }" @click="pickDate(date)">
                    {{ date | date }}
                </span>
            </div>
        </div>
        <div class="monthpicker" v-show="mode == 'month'">
            <div v-for="month in months" :key="month.getTime()">
                <span :class="{ red : isSameMonth(month, today), selected: isSameMonth(month, selected) }" @click="current = month; mode='date'">
                    {{ month | month }}
                </span>
            </div>
        </div>
        <div class="yearpicker" v-show="mode == 'year'">
            <div v-for="year in years" :key="year.getTime()">
                <span :class="{ gray : !isSameRamge(year, current), red : isSameYear(year, today), selected: isSameYear(year, selected) }" @click="current = year; mode='month'">
                    {{ year | year }}
                </span>
            </div>
        </div>
    </div>
</template>

<script>
var weeks = ["Su", "Mo", "Tu", "We", "Th", "Fr", "Sa"];
var months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
export default {
    name: "Calendar",
    data: function(){
        return {
            mode: "date", // date/month/year
            modes: ["date", "month", "year"],
            selected: null,
            current: new Date(),
            today: new Date(),
            weeks: weeks,
        }
    },
    computed: {
        dates: function(){
            var currentMonth = this.current.getMonth();
            var currentDate = this.current.getDate();
            var currentDayOfWeek = this.current.getDay();
            var ans = [];

            var begin = new Date(this.current);
            begin.setDate(1);

            // last month
            for (var i = 0; i < begin.getDay(); i++) {
                d = new Date(begin);
                d.setDate(d.getDate() - begin.getDay() + i);
                ans.push(d);
            }

            // this month
            for (var d = new Date(begin); d.getMonth() == begin.getMonth(); ) {
                ans.push(d);
                d = new Date(d);
                d.setDate(d.getDate() + 1)
            }

            // next month
            begin.setMonth(begin.getMonth() + 1);
            for (var d = new Date(begin); d.getDay() != 0 && d.getDay() < 7; ) {
                ans.push(d);
                d = new Date(d);
                d.setDate(d.getDate() + 1)
            }

            return ans;
        },
        months: function() {
            var ans = [];

            for (var i = 0; i < 12; i++) {
                var d = new Date(this.current);
                d.setMonth(i);
                ans.push(d);
            }

            return ans;
        },
        years: function() {

            var begin = new Date(this.current);
            begin.setFullYear(begin.getFullYear() - begin.getFullYear()%10 - 1);

            var ans = [];

            for (var i = 0; i < 12; i++) {
                var d = new Date(begin);
                d.setFullYear(d.getFullYear() + i);
                ans.push(d);
            }

            return ans;
        },
    },
    filters: {
        date: function(date) {
            return date.getDate();
        },
        month: function(date) {
            return months[date.getMonth()];
        },
        year: function(date) {
            return date.getFullYear();
        },
        yearRange: function(date) {
            var current = date.getFullYear();
            var begin = current - current%10;
            var end = begin + 9;

            return begin + " - " + end;
        },
    },
    methods: {
        isSameDate: function(d1, d2) {
            if (!d1 || !d2) {
                return false;
            }
            return d1.getFullYear() == d2.getFullYear() && d1.getMonth() == d2.getMonth() && d1.getDate() == d2.getDate();
        },
        isSameMonth: function(d1, d2) {
            if (!d1 || !d2) {
                return false;
            }
            return d1.getFullYear() == d2.getFullYear() && d1.getMonth() == d2.getMonth();
        },
        isSameYear: function(d1, d2) {
            if (!d1 || !d2) {
                return false;
            }
            return d1.getFullYear() == d2.getFullYear();
        },
        isSameRamge: function(d1, d2) {
            if (!d1 || !d2) {
                return false;
            }
            var current = d2.getFullYear();
            var begin = current - current%10;
            var end = begin + 9;

            return d1.getFullYear() >= begin && d1.getFullYear() <= end;
        },
        last: function() {
            if (this.mode == "date") {
                this.lastMonth();
            }
            if (this.mode == "month") {
                this.lastYear();
            }
            if (this.mode == "year") {
                this.lastRange();
            }
        },
        next: function() {
            if (this.mode == "date") {
                this.nextMonth();
            }
            if (this.mode == "month") {
                this.nextYear();
            }
            if (this.mode == "year") {
                this.nextRange();
            }
        },
        lastMonth: function() {
            var d = new Date(this.current);
            d.setMonth(d.getMonth() - 1);
            this.current = d;
        },
        nextMonth: function() {
            var d = new Date(this.current);
            d.setMonth(d.getMonth() + 1);
            this.current = d;
        },
        lastYear: function() {
            var d = new Date(this.current);
            d.setFullYear(d.getFullYear() - 1);
            this.current = d;
        },
        nextYear: function() {
            var d = new Date(this.current);
            d.setFullYear(d.getFullYear() + 1);
            this.current = d;
        },
        lastRange: function() {
            var d = new Date(this.current);
            d.setFullYear(d.getFullYear() - 10);
            this.current = d;
        },
        nextRange: function() {
            var d = new Date(this.current);
            d.setFullYear(d.getFullYear() + 10);
            this.current = d;
        },
        pickDate: function(date) {
            this.selected = date;
            this.$emit('onSelect', date);
            this.onSelect(date);
        },
        onSelect: function() {

        },
        date: function() {
            return this.selected;
        },
    },
}
</script>

<style lang="less" scoped>
@import '~font-awesome/css/font-awesome.min.css';

@white: #ffffff;
@gray: #eeeeee;
@red: #db3d44;

.calendar {
    width: 400px;
    border: 1px solid @gray;
    box-shadow: 1px 1px 5px @gray;
    text-align: center;
    border-radius: 5px;
    padding: 15px;

    @media (max-width: 320px) {
        width: 100%;
        padding: 5px;
    }

    .controller {
        padding: 5px 5px 10px 5px;
        div {
            cursor: pointer;
        }
    }

    .gray {
        color: @gray;
    }

    .red {
        color: @red;
    }

    .selected {
        color: @white;
        background-color: @red;
    }

    span {
        padding: 5px 8px;
        border-radius: 100%;
        cursor: pointer;
    }

    .datepicker {
        display: grid;
        grid-template-areas: "a b c d e f g";
        
        .week-day {
            font-weight: bold;
        }

        div {
            padding: 5px;
        }

    }

    .monthpicker, .yearpicker {
        display: grid;
        grid-template-areas: "a b c d";
        
        div {
            padding: 20px 5px;
        }

    }

}

</style>
