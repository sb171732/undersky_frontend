<template>
  <div class="test w3-padding" >
<!--    <img alt="Vue logo" src="../assets/logo.png">-->
<!--    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
  <h3>Региональный исторический диктант, посвященный 100-летию образования Тувинской Народной Республики</h3>
  
  <el-button class="w3-margin-bottom" @click="gettest">Начать тест</el-button>
  <div class="w3-margin">
  </div>
   <el-timeline>
    <el-timeline-item v-for="q in questions" :key="q.id" :timestamp="'Вопрос #'+q.num" placement="top" color="#67C23A">
      <el-card>
            <h4 class="q" v-html='q.q_text'></h4>
<el-radio-group v-model="q.radio" size='medium'>
<div class="w3-row ">
	<div class="w3-col w3-margin" v-for="(a,index) in even(q.num)" :key="index">
		<el-radio border   :label="a.id">{{a.a_text}}</el-radio>
	</div>
</div>
</el-radio-group>
      </el-card>
    </el-timeline-item>
    
  </el-timeline>
  <el-button type="success" icon="el-icon-check" round @click="end">Завершить тест</el-button>
  
  
  
  
  <el-dialog
  title="Результаты"
  :visible.sync="dialogVisible"
  width="60%"
  >
  <table class="w3-table">
 <tr v-for="q in questions" :key="q.num">
 <td v-html="q.num"></td>
 <td v-if="q.a_true_id===q.radio" class="w3-text-green">Верно</td>
 <td v-else class="w3-text-red">Неверно</td>
 </tr>
  </table>
  <h4 class="w3-pading-top" style="text-align:center;width:100%">Набрано баллов: {{ col }}  </h4>
  <el-progress type="circle" :percentage="Math.round(col*100/10)"></el-progress>
  <span slot="footer" class="dialog-footer">
    
    <el-button type="primary" @click="$router.push('/')">Выйти</el-button>
  </span>
</el-dialog>
  </div>
</template>
<script src="https://ioko.rtyva.ru/CDN/cookie.js"></script>
<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

import axios from 'axios';	
	
export default {
  name: 'Test',
  data(){
	return {
		dialogVisible:false,
		questions:[],
		answers:[],
		col:0,
		code:[]
	}
    
  },
computed:{
answ:function(){ return this.answers }	
},	
methods: {
end(){
	var m='false'
	for (var q in this.questions){
		if (this.questions[q].radio==='0'){ m='true'}}
	if (m==='false'){this.dialogVisible = true
	let col=0
	for (let w of this.questions){
		if (w.radio===w.a_true_id){ col++}
	} 		this.col=col
	axios.post('http://95.167.178.158/other/projects/undersky/rest.php','&save_res='+col+'&u_id='+getCookie('user_id')).then((response) => { console.log(response) }).catch((error) => { console.log(error); });	
}
	else {  this.$message({
          message: 'Ответьте на все вопросы теста!',
          type: 'warning'
        });
 }
	console.log(m)

	
	
},	
  even:function(p){
return this.answers.filter(function(n){
	return n.q_id===p
})
  },gettest(){
console.log('')
  }
},
created(){
	console.log(getCookie('code'))
	console.log(getCookie('user_id'))
	if (getCookie('code')===undefined) { this.$router.push('/') }
   else{
axios.post('http://95.167.178.158/other/projects/undersky/rest.php','check_user='+getCookie('code')+'&u_id='+getCookie('user_id')).then((response) => {
		if (response!='succes'){ this.$route.push('/') }
}).catch((error) => { console.log(error); });	
	
	
	
	axios.post('http://95.167.178.158/other/projects/undersky/rest.php','&get_q=').then((response) => {
		this.questions=response.data,console.log(response.data[4])
}).catch((error) => { console.log(error); });	
	axios.post('http://95.167.178.158/other/projects/undersky/rest.php','&get_a=').then((response) => {
		this.answers=response.data,console.log(response.data[4])
})
.catch((error) => {
				console.log(error);
});	 
	
	
}	}
}
</script>
<style>
	.q{ text-align: initial; }
	.el-radio-group{
		width: -webkit-fill-available;
	}
	.el-radio{
		width: 100%;
    text-align: initial;
	}	
  .el-carousel__item h3 {
    color: #475669;
    font-size: 18px;
    opacity: 0.75;
    line-height: 300px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
	.test{margin-bottom: 260px;}
</style>
