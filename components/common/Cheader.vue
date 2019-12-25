<template>
    <div class="header">
        <div class="logo">银行储蓄系统</div>
        <div class="identification">客户端</div>
        <div class="app">{{formatDate(date)}}</div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                date:new Date()
            }
        },
        computed:{
            username(){
                let username = sessionStorage.getItem('ms_username');
                return username ? username : this.name;
            }
        },
        mounted() {
            let _this = this; // 声明一个变量指向Vue实例this，保证作用域一致
            this.timer = setInterval(() => {
            _this.date = new Date(); // 修改数据date
            }, 1000)
        },
        beforeDestroy() {
            if (this.timer) {
            clearInterval(this.timer); // 在Vue实例销毁前，清除我们的定时器
            }
        },
        methods: {
            formatDate(date) {
            return date.toLocaleString("zh-Hans-CN", {
            // timeZone: ['UTC'],
            hour12: false,
            year: "numeric",
            month: "2-digit",
            day: "2-digit",
            hour: "2-digit",
            minute: "2-digit",
            second: "2-digit"
        });
        }
    },
}
</script>
<style scoped>
    .header {
        position: relative;
        box-sizing: border-box;
        width: 100%;
        height: 70px;
        background-color:rgb(32, 41, 53);;
        font-size: 22px;
        line-height: 70px;
    }
    .header .logo{
        float: left;
        width:250px;
        text-align: center;
        color: rgb(255, 255, 255);
    }
    .header .identification{
        float: left;
        font-size: 16px;
        color: rgb(255, 255, 255);
        text-align: center;
    }
    .header .app{
        float: right;
        width:250px;
        font-size: 16px;
        color: rgb(255, 255, 255);
        text-align: center;
    }
</style>
