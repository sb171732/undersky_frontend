<template>
  <div id="app">
    <img src="./assets/logo.png" width="100%">
    <div>
    <p>{{ $route.path }}</p>
<el-menu v-if="$route.path==='/'" default-active="1" class="el-menu-demo" mode="horizontal" @select="handleSelect" router>
  <el-menu-item index="1" route="/">Регистрация / Вход</el-menu-item>
  <el-menu-item index="2" route="/test" disabled>Тестирование</el-menu-item>
  <el-menu-item index="3" disabled>Узнать результаты теста</el-menu-item>
</el-menu>
<el-menu v-if="$route.path==='/test'" default-active="2" class="el-menu-demo" mode="horizontal" @select="handleSelect" router>
  <el-menu-item index="1" route="/">Регистрация / Вход</el-menu-item>
  <el-menu-item index="2" route="/test">Тестирование</el-menu-item>
  <el-menu-item index="3" disabled>Узнать результаты теста</el-menu-item>
</el-menu>
    
     
       
    
    <router-view/>   
    </div>
<!--    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'

export default {	
  name: 'app',
  data(){
return {
activeIndex: '1', 
per: '1', 
  }

  },
components: {
//    HelloWorld
  },
created(){
// console.log(this.$router.path)
	
	
  },
 methods:{
handleSelect() {
//console.log(key, keyPath);
}
 }	
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
