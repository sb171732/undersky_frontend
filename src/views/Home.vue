<template>
  <div class="home">
<!--    <img alt="Vue logo" src="../assets/logo.png">-->
<!--    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
  <h3>Для тестирования потребуется email </h3>

    {{ user_code }}
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
   input_fio:'asdasd',
   input_email:'sauronkan@gmail.com',
   sel_cat:'',
   code_confirm:'',		
   user_code: 0 ,		
   user_id:''		
	}
  }
	,
 methods:{
save_user(){
 axios.post('https://test-undersky.rtyva.ru/rest.php','save_user='+this.input_fio+'&email='+this.input_email+'&userType='+this.sel_cat+'&code='+this.code).then((response) => {
console.log(response)
}).catch((error) => { console.log(error); });
 },	 
 confirm(){
	if (this.user_code!=this.code_confirm){ 
this.$message({showClose: true,message: 'Код неверен!',type: 'warning'});   }
else{ this.$router.push('/test'),setCookie('code',this.code),setCookie('user_id',this.user_id) }
 },
 send(){
         //api.rtyva.ru/api/feedback/register
   fetch('https://api.rtyva.ru/api/feedback/register',{
       method: "POST",
       headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
    },
       body: JSON.stringify({fio: this.input_fio, userType: this.sel_cat,email:this.input_email})
     })
 .then(response => response.json())
  .then(result =>{
    console.log(result)
    if (result.error===true){ 
          this.$message({
          message: result.reason,
          type: 'warning'
        });
    } else {
       this.user_code = result.code , this.user_id=result.id, this.reg1=false, this.reg2=true
    }
    // 
    }  );
  // 
    
    // console.log(user.id)

 }
 }	
}


</script>
<style>
	.home{
		padding-bottom: 200px;
	}

</style>
