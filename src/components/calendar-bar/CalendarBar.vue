<template>
    <div class="calendar-bar">
        <div class="top-time">
            <div class="now-time">{{nowTime}}</div>
            <div class="now-day" @click="backToday">{{year}}年{{month}}月{{date}}日</div>
        </div>
    </div>
</template>

<script>
    import {eventBus} from "../../main";

    export default {
        name: "CalendarBar",
        data(){
            return{
                times:new Date(),
                year:new Date().getFullYear(),
                month:new Date().getMonth()+1,
                date:new Date().getDate(),
                nowTime:''
            }
        },
        methods: {
            currentTime() {
                this.getDate();
                setInterval(this.getDate, 1000);
            },
            getDate() {
                let _this = this;

                let hh = new Date().getHours();
                let mf = new Date().getMinutes() < 10 ? "0" + new Date().getMinutes() : new Date().getMinutes();
                let ss = new Date().getSeconds() < 10 ? "0" + new Date().getSeconds() : new Date().getSeconds();

                _this.nowTime = hh + ":" + mf+":"+ss;
            },
            //兄弟组件监听
            backToday(){
                eventBus.$emit("abc");
                // console.log(this.times)
            }
        },
        mounted() {
            this.currentTime();
        },
        // 销毁定时器
        beforeDestroy: function() {
            if (this.getDate) {
                clearInterval(this.getDate);
            }
        }

    }
</script>

<style scoped>
    .calendar-bar{
        display: flex;
        height: 120px;
    }
    .top-time{
        align-self: center;
        margin-left: 20px;
    }
    .now-time{
        font-size: 40px;
        color: #e6e6e6;
    }
    .now-day{
        margin-left: 5px;
        color: #1296db;
        cursor: pointer;
    }
    .now-day:hover{
        color: aqua;
    }
</style>