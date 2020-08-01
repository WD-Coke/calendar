<template>
    <div>
        <div id="calendar-header">
            <div class="year-month" @click="goMonth">
                <div id="ym">
                    {{year}}年{{month}}月
                </div>
            </div>
            <div class="pre" @click="goPre"><slot name="pre"></slot></div>
            <div class="next" @click="goNext"><slot name="next"></slot></div>
        </div>
    </div>
</template>

<script>
    import {eventBus} from "../../main";
    export default {
        name: "CalendarHeader",
        data(){
            return {
                year:new Date().getFullYear(),
                month:new Date().getMonth()+1,
            }
        },
        methods:{
            goPre(){
                eventBus.$emit("goPre");
            },
            goNext(){
                eventBus.$emit("goNext");
            },
            goMonth(){
                eventBus.$emit("goMonth")
            }
        },
        created() {
            eventBus.$on('goPreBack',res=>{
                this.year=res[0];
                this.month=res[1];
            });
            eventBus.$on('goNextBack',res=>{
                this.year=res[0];
                this.month=res[1];
            });
            eventBus.$on('goMonthBack',res=>{
                this.year=res[0];
                this.month=res[1];
            })
        }

    }
</script>

<style scoped>
    #calendar-header{
        display: flex;
        height: 64px;
        line-height: 65px;
        color: #e6e6e6;
        border-top: 1px solid #8a8a8a;
    }
    #ym:hover{
        color: #ffffff;
    }
    .pre,.next{
        width: 14%;
        text-align: center;
    }
    .year-month{
        flex: 1;
        padding-left: 25px;
    }
    #ym{
        width: 40%;
        cursor: pointer;
    }
    #calendar-header img{
        width: 30px;
        height: 30px;
        margin-top: 10px;
        margin-bottom: -10px;
    }
</style>