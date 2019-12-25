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
                        icon: 'el-icon-user',
                        index: 'Sinfo',
                        title: '个人中心'
                    },
                    {
                        icon: 'el-icon-document',
                        index: 'Ssearch_deposit',
                        title: '存款记录查询'
                        
                    },
                    {
                        icon: 'el-icon-document',
                        index: 'Ssearch_withdrawal',
                        title: '取款记录查询'
                    },
                    {
                        icon: 'el-icon-document-add',
                        index: 'Sdeposit',
                        title: '存款业务办理'
                        
                    },
                    {
                        icon: 'el-icon-document-add',
                        index: 'Swithdrawal',
                        title: '取款业务办理'
                    },
                    {
                        icon: 'el-icon-printer',
                        index: 'Sinterest',
                        title: '利息清单打印'
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
