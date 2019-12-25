<template>
    <div class="wrapper">
        <v-head></v-head>
        <v-sidebar></v-sidebar>
        <div class="content">
            <transition name="move" mode="out-in"><router-view></router-view></transition>
        </div>
    </div>
</template>

<script>
    import vHead from './Aheader.vue';
    import vSidebar from './Asidebar.vue';
    export default {
        components:{
            vHead, vSidebar
        }
    }
</script>