<template>
        <div>
            <div v-if="change">
                <div class="calendar-body-top">
                    <div class="calendar-body-week" v-for="week in weekday">{{week}}</div>
                </div>
                <div class="calendar-body-a">
                    <div id="calendar-body-beforeDay" v-for="day in beforeDay">{{day}}</div>
                    <div id="calendar-body-today" :tabindex="index" @click="" v-for="(day,index) in toDate" :class="{today:today===day&&time.getMonth()===toMoth&&time.getFullYear()===toYear}">{{day}}</div>
                    <div id="calendar-body-afterDay" v-for="day in afterDay">{{day}}</div>
                </div>
            </div>

            <div v-else class="calendar-body-m">
                <div v-for="(month,index) in monthInYear" id="calendar-body-month" :class="{m:index===toMoth}" @click="jump(index)">{{month}}</div>
            </div>
        </div>
</template>

<script>
    import {eventBus} from "../../main";

    export default {
        name: "CalendarBody",
        data(){
            return{
                year:"",
                month:"",
                weekday: ["一", "二", "三", "四", "五", "六", "日"],
                daysInMonth: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
                monthInYear:["一月","二月","三月","四月","五月","六月","七月","八月","九月","十月","十一月","十二月"],
                time:new Date(),
                beforeDay:[],
                toDate:0,
                afterDay:[],
                change:true,
                currentIndex:0
            }
        },
        mounted() {
            this.dayInit();
        },
        computed: {
            today(){
              return new Date().getDate();
            },
            toMoth(){
                return new Date().getMonth();
            },
            toYear(){
                return new Date().getFullYear();
            },
        },
        methods:{
            beforeDays() {
                let firstDay = this.time;
                firstDay.setDate(1);
                let firstWeek = firstDay.getDay();
                if (firstWeek === 0) {
                    firstWeek = 7;
                }
                for (let i = 0; i < firstWeek - 1; i++) {
                    firstDay.setDate(firstDay.getDate() - 1);
                    this.beforeDay.unshift(firstDay.getDate());
                }
                firstDay.setDate(firstDay.getDate() + firstWeek - 1);
                firstDay.setDate(this.today);
            //     if (this.time.getMonth() !== 1) {
            //         if (this.today === 31) {
            //             firstDay.setDate(this.today - 1);
            //         } else {
            //             firstDay.setDate(this.today);
            //         }
            //
            //     } else {
            //         if (this.daysInMonth[1] === 28) {
            //             firstDay.setDate(this.today - 3)
            //         } else {
            //             firstDay.setDate(this.today - 2)
            //         }
            //     }
            },
            toDates(){
                let today=0

                if ((this.year % 4 === 0 && this.year % 100 !== 0) || this.year % 400 === 0) {
                    this.daysInMonth[1] = 29;
                }
                else {
                    this.daysInMonth[1]=28;
                }
                today=this.daysInMonth[this.time.getMonth()];
                this.toDate=today;
            },
            afterDays(){
                console.log(this.time.getMonth())
                for(let i=0;i<42-this.daysInMonth[this.time.getMonth()]-this.beforeDay.length;i++){
                    this.afterDay.push(i+1);
                }
            },
            dayClear(){
                this.beforeDay=[];
                this.toDate=0;
                this.afterDay=[];
                this.toDates();
                this.beforeDays();
                this.afterDays();
            },
            dayInit(){
                this.year="";
                this.month="";
                this.year=this.time.getFullYear();
                this.month=this.time.getMonth()+1;
            },
            goPre(){
                this.time.setDate(1)
                this.time.setMonth(this.time.getMonth()-1)
                this.dayClear();
                this.dayInit();
                eventBus.$emit("goPreBack",[this.year,this.month])
            },
            goToday(){
                this.time.setDate(this.today)
                this.time.setMonth(this.toMoth)
                this.dayClear();
                this.dayInit();
            },
            goNext(){
                this.time.setDate(1);
                this.time.setMonth(this.time.getMonth()+1);
                this.dayClear();
                this.dayInit();
                eventBus.$emit("goNextBack",[this.year,this.month])
                console.log(this.time.getMonth())
            },
            jump(index){
                this.currentIndex=index;
                this.time.setDate(1);
                this.time.setMonth(this.currentIndex);
                this.dayClear();
                this.dayInit();
                this.change=!this.change;
                eventBus.$emit('goMonthBack',[this.year,this.month])
            },
        },
        created() {
            eventBus.$on('abc', (message) => {
                this.goToday();
            })
            eventBus.$on('goPre',res=>{
                this.goPre();
            })
            eventBus.$on('goNext',res=>{
                this.goNext();
            })
            eventBus.$on('goMonth',res=>{
                this.change=!this.change;
            })
            this.dayClear();
        }
    }
</script>

<style scoped>
    .calendar-body-top{
        display: flex;
        height: 50px;
        justify-content: space-between;
        margin-bottom: -10px;
    }
    .calendar-body-week{
        width: 14%;
        height: 50px;
        text-align: center;
        color: #e6e6e6;
    }



    .calendar-body-a,.calendar-body-m{
        display: flex;
        flex-wrap: wrap;
        height: 300px;
        justify-content: space-between;
        align-items:center;
    }
    #calendar-body-today, #calendar-body-beforeDay,#calendar-body-afterDay{
        width: 12%;
        height: 12%;
        text-align: center;
        border: 2px solid rgba(158,158,158,0);
    }
    #calendar-body-today {
        color: #e6e6e6;
    }
    #calendar-body-beforeDay,#calendar-body-afterDay {
        color: #8a8a8a;
    }
    #calendar-body-today:hover,#calendar-body-beforeDay:hover,#calendar-body-afterDay:hover{
        border: 2px solid rgba(158,158,158,1);
    }
    #calendar-body-today:focus{
        border: 2px solid;
    }
    .calendar-body-a .today{
        background-color: #1296db;
    }

    #calendar-body-month{
        width: 32%;
        height: 16%;
        padding-top: 6%;
        text-align: center;
        color: #e6e6e6;
        border: 2px solid rgba(158,158,158,0);
    }
    #calendar-body-month:hover{
        border: 2px solid rgba(158,158,158,1);
    }
    .calendar-body-m .m{
        background-color: #1296db;
    }
</style>