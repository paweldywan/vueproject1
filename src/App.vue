<template>
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" />
    <OpenLayersWmts />
    <OpenLayersDemo />
</template>

<script lang="ts">
    import { defineComponent } from 'vue';
    import HelloWorld from './components/HelloWorld.vue';
    //import OpenLayersDemo from './components/OpenLayersDemo.vue';
    //import OpenLayersWmts from './components/OpenLayersWmts.vue';

    export default defineComponent({
        name: 'App',
        components: {
            HelloWorld//,
            //OpenLayersDemo,
            //OpenLayersWmts
        }
    });
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
