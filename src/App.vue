<template>

  
    <div id="app">
	  
	  <div  v-if="!visible">
		  <ul>

		<li>100057489079 <button type="primary" @click="handleClick('100057489079')">3个规格加强版示例</button></li>

	   <li><input v-model="skuInfo" />
	   <button @click="handleClick(skuInfo)">打开指定skuId示例</button></li>
	   </ul>
	    </div>
	    <new-sku-two v-if="visible" :visible.sync="visible" :skuInfo="skuInfo"/>
	  
	  <br>本代码改编自参考地址 https://blog.csdn.net/baidu_16839249/article/details/114370304#t4 


	  
    </div>

  

</template>

<script>

import newSkuTwo from '@/components/newSkuTwo.vue'
import axios from 'axios';
export default {
  components: {
	newSkuTwo
  },
  data(){
	  return{
		  showSkuSelect:false,
		  showSkuButton:true,
		  visible:false,
		  skuId9:'',
		  skuInfo:'100057489079'
	  }
	  },
  methods:{
	handleClick(val){
		console.log('-->>',(val).toString());
this.skuInfo=val;
this.visible=true;


  }
}
}
</script>