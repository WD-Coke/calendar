<template>
        <!--日历容器-->
        <div class="calendar">
            <!--顶部时间栏-->
            <CalendarBar></CalendarBar>
            <CalendarHeader>
                <div slot="pre">
                    <img src="../../src/assets/img/next.svg">
                </div>
                <div slot="next">
                    <img src="../../src/assets/img/pre.svg">
                </div>
            </CalendarHeader>
            <!--日期展示栏-->
            <CalendarBody></CalendarBody>
        </div>
</template>

<script>
    import CalendarBody from "./calendar-body/CalendarBody";
    import CalendarBar from "./calendar-bar/CalendarBar";
    import CalendarHeader from "./calendar-header/CalendarHeader";

    export default {
        name: "calendar",
        components:{
            CalendarHeader,
            CalendarBar,
            CalendarBody
        },
    }
</script>

<style scoped>
    .calendar{
        display: block;
        width: 350px;
        height: 540px;
        background-color: rgba(0,0,0,.7);
        box-shadow: 3px 3px 3px rgba(100,100,100, 0.5);
    }
</style>