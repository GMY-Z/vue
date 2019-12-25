<template>
    <div class="sidebar">
        <el-menu :default-active="onRoutes" class="el-menu-vertical-demo" theme="dark" unique-opened router>
            <template v-for="item in items">
                <el-menu-item :index="item.index">
                    <i :class="item.icon"></i>{{ item.title }}
                </el-menu-item>
            </template>
        </el-menu>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                items: [
                    {
                        icon: 'el-icon-bank-card',
                        index: 'Abusiness',
                        title: '银行业务总览'          
                    },
                    {
                        icon: 'el-icon-info',
                        index: 'Ainfo_manage',
                        title: '业务员信息管理'
                    },
                    {
                        icon: 'el-icon-money',
                        index: 'Arate_manage',
                        title: '利率管理'
                    },
                    {
                        icon: 'el-icon-document',
                        index: 'Asearch_deposit',
                        title: '存款记录查询' 
                    },
                    {
                        icon: 'el-icon-document',
                        index: 'Asearch_withdrawal',
                        title: '取款记录查询'
                    },
                    {
                        icon: 'el-icon-switch-button',
                        index: 'login',
                        title: '退出'
                    }
                ]
            }
        },
        computed:{
            onRoutes(){
                return this.$route.path.replace('/','');
            }
        }
    }
</script>

<style scoped>
    .sidebar{
        display: block;
        position: absolute;
        width: 250px;
        left: 0;
        top: 70px;
        bottom:0;
        background: #2E363F;
    }
    .sidebar > ul {
        height:100%;
    }
</style>
