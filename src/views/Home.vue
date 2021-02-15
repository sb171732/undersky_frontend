<template>
  <div class="home">
<!--    <img alt="Vue logo" src="../assets/logo.png">-->
<!--    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
  <h3>Для тестирования потребуется email </h3>
   <div class="" v-if="reg1">
 <el-select class="w3-margin-bottom" v-model="sel_cat" placeholder="Выбрать категорию">
    <el-option
     label="Ученик"
     value="uch">
    </el-option>
    <el-option
     label="Учитель"
     value="teach">
    </el-option>
    <el-option
     label="Не в школе"
     value="other">
    </el-option>
  </el-select><br>
  <el-input placeholder="ФИО" type="text" v-model="input_fio" style="width:220px"></el-input><br>
  <el-input placeholder="email" type="text" v-model="input_email" style="width:220px" class="w3-margin-top"></el-input><br>
  <el-button class="w3-margin-top" type="primary" plain @click="send()">Вход</el-button>
  
   </div>
  <div class="" v-if="reg2">
	<el-input placeholder="Введите код из почты" type="text" v-model="code_confirm" style="width:220px" class="w3-margin-top"></el-input><br>
  <el-button class="w3-margin-top" type="primary" plain @click="confirm()">Подтвердить почту</el-button>
  </div>
  
  
  </div>
</template>
<script src="https://ioko.rtyva.ru/CDN/cookie.js"></script>
<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'
	
export default {
  name: 'Home',
  data(){
	return  {
   reg1:true,
   reg2:false,
   input_fio:'',
   input_email:'',
   sel_cat:'',
   code_confirm:'',		
   code:'',		
   user_id:''		
	}
  }
	,
 methods:{
save_user(){
 axios.post('http://95.167.178.158/other/projects/undersky/rest.php','save_user='+this.input_fio+'&email='+this.input_email+'&userType='+this.sel_cat+'&code='+this.code).then((response) => {
console.log(response)
}).catch((error) => { console.log(error); });
 },	 
 confirm(){
	if (this.code!=this.code_confirm){ 
this.$message({showClose: true,message: 'Код неверен!',type: 'warning'});   }
else{ this.$router.push('/test'),setCookie('code',this.code),setCookie('user_id',this.user_id) }
 },
 send(){

 axios.post('http://95.167.178.110:9987/api/feedback/register','fio='+this.input_fio+'&email='+this.input_email+'&userType='+this.sel_cat).then((response) => {
	this.code=response.data.code,this.user_id=response.data.id, this.reg1=false,this.reg2=true
//	this.save_user()
}).catch((error) => { console.log(error); });	
 }
 }	
}








</script>
<style>
	.home{
		padding-bottom: 200px;
	}

</style>
