<!-- Use preprocessors via the lang attribute! e.g. <template lang="pug"> -->
<template>
  <div >
       
        <div id="sku" v-if="key.length>0" v-for="(keyItem,index) in key">
            <div class="title">规格维度-{{keyItem.name}} :</div>
            <div v-for="(label,sort) in keyItem.item" :class="['label-info', {'selected':keyItem.itemIndex===sort}]" style="display: inline-block;">
                                <button @click="toggeItem(index, sort)" :disabled="!checkSku(label,index)" :class="{disabled: !checkSku(label,index)}">
								<img v-if="keyItem.itemImage[sort].length>0" :src=keyItem.itemImage[sort] :class="['label_img']"/>
								{{label}}</button>

            </div>
        </div>
        <div id="result" v-if="result">
            <div>假库存：{{result.count}}</div>
            <div>假价格：{{result.price}}</div>
			<div>skuid：{{result.skuid}}</div>
			<div><img :src=result.imagePath style="width:200px;" /></div>
        </div>
		
		<div style="margin-top:1px;" :class="{'selected':canBuy===0}">
			<button @click="toggleDialog('dialogVisible', false)">关闭本页</button>
			<button @click="myChoose(result)">{{canBuy===0?"请选择规格":"加入购物车"}}</button>
		</div>
   </div>
</template>

<script>
import axios from 'axios';
export default {
	props: {
	        visible: Boolean,
			skuId9:String,
			skuInfo:String
	    },
  data(){
	  return{
      message: 'Hello Vue!',
	  canBuy:0,
	  key:[],
	  sku:{},
	  skuDetail:{
	"sku": {
		"新【小米平板6】远山蓝;8+256GB/144Hz/2.8K;官方标配": {
			"skuid": "100056375397",
			"price": 496,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+256GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100050512106",
			"price": 205,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+128GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100057489095",
			"price": 194,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+256GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100050512090",
			"price": 189,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+128GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100057489061",
			"price": 160,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+128GB/144Hz/2.8K;官方标配": {
			"skuid": "100048837216",
			"price": 315,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+256GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100050512182",
			"price": 281,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+128GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100057489145",
			"price": 244,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+256GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100050512116",
			"price": 215,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;8+128GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100057489113",
			"price": 212,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】远山蓝;6+128GB/144Hz/2.8K;官方标配": {
			"skuid": "100048837194",
			"price": 293,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;6+128GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100050512078",
			"price": 177,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+128GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100057489079",
			"price": 178,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+256GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100057489143",
			"price": 242,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+128GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100050512074",
			"price": 173,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+256GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100057489077",
			"price": 176,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+256GB/144Hz/2.8K;官方标配": {
			"skuid": "100048837232",
			"price": 331,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+128GB/144Hz/2.8K;官方标配": {
			"skuid": "100048837214",
			"price": 313,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+256GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100050512100",
			"price": 199,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+256GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100050512094",
			"price": 193,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;6+128GB/144Hz/2.8K;官方标配": {
			"skuid": "100048837218",
			"price": 317,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;6+128GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100050512152",
			"price": 251,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+128GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100057489067",
			"price": 166,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;6+128GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100057489129",
			"price": 228,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;8+128GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100057489103",
			"price": 202,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】金色;6+128GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100050512176",
			"price": 275,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+256GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100050512174",
			"price": 273,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+256GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100050512172",
			"price": 271,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+128GB/144Hz/2.8K;触控笔套装": {
			"skuid": "100050512140",
			"price": 239,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;6+128GB/144Hz/2.8K;官方标配": {
			"skuid": "100056375399",
			"price": 498,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+128GB/144Hz/2.8K;官方标配": {
			"skuid": "100056375377",
			"price": 476,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+256GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100057489065",
			"price": 164,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+128GB/144Hz/2.8K;标准键盘套装": {
			"skuid": "100050512148",
			"price": 247,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+256GB/144Hz/2.8K;官方标配": {
			"skuid": "100048837212",
			"price": 311,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+256GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100057489085",
			"price": 184,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+128GB/144Hz/2.8K;触控键盘套装": {
			"skuid": "100050512112",
			"price": 211,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		},
		"新【小米平板6】黑色;8+128GB/144Hz/2.8K;触控键盘+触控笔套装": {
			"skuid": "100050512080",
			"price": 179,
			"count": 1,
			"imagePath": "//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
		}
	},
	"key": [{
			"name": "颜色",
			"itemIndex": 1,
			"item": [
				"新【小米平板6】远山蓝",
				"新【小米平板6】金色",
				"新【小米平板6】黑色"
			],
			"imagePath": "",
			"itemImage": [
				"//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg",
				"//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg",
				"//img13.360buyimg.com/n0/jfs/t1/135050/18/30124/71701/643e634eF33711340/1bcf1ef8286605b7.jpg"
			]
		},
		{
			"name": "存储",
			"itemIndex": 1,
			"item": [
				"6+128GB/144Hz/2.8K",
				"8+128GB/144Hz/2.8K",
				"8+256GB/144Hz/2.8K"
			],
			"imagePath": "",
			"itemImage": [
				"",
				"",
				""
			]
		},
		{
			"name": "套装组合",
			"itemIndex": 4,
			"item": [
				"官方标配",
				"标准键盘套装",
				"触控笔套装",
				"触控键盘+触控笔套装",
				"触控键盘套装"
			],
			"imagePath": "",
			"itemImage": [
				"",
				"",
				"",
				"",
				""
			]
		}
	],
	"code": 0
},
      // 重新组合的sku数据
      resultSKU: []
	}
    },computed: {
	  
	  dialogVisible: {
	              get() {
	                  return this.visible;
	              },
	              set(val) {
	                  this.$emit('update:visible', val);
	              },
	          },
			  },
    mounted() {
		//this.getSkuDetailInfo('100005886305');
		this.getSkuDetailInfo(this.skuInfo);
        //this.initSku();
    },
    methods: {
        initSku() {
            const sku = this.sku;
            let resultSKU = [];
            let skuKeys = this.getObjKeys(sku);
            for(let i in skuKeys) {
                let skuKey = skuKeys[i]; // 获取一条SKU的key
                let skuData = sku[skuKey];  //获取当前sku的数据
				//console.log(skuData);
                let skuKeyAttrs = skuKey.split(';'); //获取当前sku的属性数组
                let combArr = this.recombine(skuKeyAttrs); //获取当前sku拆分的组合数组               
                for(let j=0;j<combArr.length;j++) {
                    let key = combArr[j].join(';');
                    if(resultSKU[key]) {
                        resultSKU[key].count += skuData.count;
                        resultSKU[key].price.push(skuData.price);
						 resultSKU[key].skuid = skuData.skuid;
                    } else {
                        resultSKU[key] = {
                            count: skuData.count,
                            price: [skuData.price],
							skuid: skuData.skuid,
							imagePath:skuData.imagePath,
                        }
                    }
                }
            }
			this.showCountAndPrice(resultSKU);//初始化选择状态
           // console.log(resultSKU, 'resultSKU');
            this.resultSKU = resultSKU;
        },
        recombine(arr) {
            const labelArr = arr;
            // 获取重新组合的数组格式 例如[['黑', ''],['16G', ''],['电信', '']]
            // 进行横向遍历，填充组合数组
            const newLabelArr = labelArr.map((item) => [item, '']);

            // 进行横向遍历，填充组合数组，从0开始递归
            let resultArr = this.getCombineArr(newLabelArr,0);
            return resultArr;
        },
        getCombineArr(arr,index) {
            let resultArr = [];
            let newArr = [];
            const recursion = (arr,index) => {
                for(let i=0;i<arr[index].length;i++) {
                    newArr[index] = arr[index][i];
                    if(index===(arr.length-1)) {
                        resultArr.push(JSON.parse(JSON.stringify(newArr)));
                    } else {
                        // 递归
                        recursion(arr,index+1)
                    }
                }
            }
            recursion(arr,index);
            return resultArr;
        },
        // 获取对象key数组集合
        getObjKeys(obj) {
            if(obj!==Object(obj)){
                throw new TypeError('Invalid object!')
            }
            let keysArr = [];
            for(let key in obj) {
                if(Object.prototype.hasOwnProperty.call(obj, key)){
                    keysArr[keysArr.length] = key;
                }
            }
            return keysArr;
        },
        toggeItem(index,sort) {
            if(this.key[index].itemIndex === sort) {
                this.key[index].itemIndex = -1;
            } else {
                this.key[index].itemIndex = sort;
            }
            this.showCountAndPrice(this.resultSKU);
        },
        showCountAndPrice(skuData) {
            let skuLabelArr = [];
			let i=0;
			let j=0;
            this.key.forEach(element => {
				j++
                const _index = element.itemIndex;
                //skuLabelArr.push(_index===-1?'':element.item[_index]);
				if (_index===-1){
					i=i;
					//skuLabelArr.push();
				}else{
					i++;//计算已选
					skuLabelArr.push(element.item[_index]);
				}
				
            });
			if (i===j){
				console.log('已选完');
				this.canBuy=1;
			}else{
				this.canBuy=0;
			} 
            const skuLabel = skuLabelArr.join(';');
			//console.log('11111');
			//console.log(skuLabel);
			//console.log(skuData);
			//let all_num = this.key.length;
			//let selected_num = skuLabel.split(';')
			
            if(skuData[skuLabel]) {
                this.result = skuData[skuLabel];
                const maxPrice = Math.max(...skuData[skuLabel].price);
                const minPrice = Math.min(...skuData[skuLabel].price);
                this.result = {
                    count: skuData[skuLabel].count,
                    price: maxPrice > minPrice ? minPrice + '-' + maxPrice : maxPrice,
					skuid:skuData[skuLabel].skuid,
					imagePath:skuData[skuLabel].imagePath
                }
            } else {
                this.result = null;
            }
        },
      checkSku(str,index) {

                let choosedLabelArr = [];
                const key = this.key;
                key.forEach((item) => {
                    choosedLabelArr.push(item.itemIndex>-1?item.item[item.itemIndex]:'')
                });

                // 假设当前已选中
                choosedLabelArr[index] = str;
                let nowChooseStr = choosedLabelArr.join(';');

                // 遍历加工完的数据集，是否有库存可选，有就可点击，没有就禁用
                let canChoose = false;
                const resultSKU = this.resultSKU;
                for(let skuLey in resultSKU) {
                    if(skuLey.indexOf(nowChooseStr)>-1) {
                        if(resultSKU[skuLey].count>0) {
                            canChoose = true;
                            break; 
                        }
                    }
                }
                return canChoose;
            },
			getSkuDetailInfo(skuId){
				 /* axios.get(`http://请求后台?action=skuInfoTb&skuId=`+skuId).then(
				         res =>{

						  if ('code' in res.data && res.data.code !=0){
							  alert(JSON.stringify(res.data));
							  this.key=[];
							  this.sku={};
							  this.toggleDialog('dialogVisible', false);
							  return false;
						  }
						  
				  		  this.key=(res.data.key);
						  this.sku=(res.data.sku);
						  this.initSku();
						  //console.log(this.sku);
						  //console.log(this.sku1);
				           //提供数据：search组件要给list组件传递数据，就要触发list组件中的自定义事件并携带要传递的数据
				           //请求成功后更新list里面的数据
				           //this.$bus.$emit("updateListDate",{isLoading:false,errMsg:'',users:res.data.items})
				         },
				         error =>{
				           console.log("请求成功",error.message)
				           //请求失败后更新list里面的数据
				           //this.$bus.$emit("updateListDate",{isLoading:false,errMsg:error.message,users:[]})
				         }
				       )*/
					   this.key=(this.skuDetail.key);
					   this.sku=(this.skuDetail.sku);
					   this.initSku();
					   
			},
			myChoose(s){
				if(!s || this.canBuy==0){
					alert('规格选择尚未结束！');
					return false;
				}
				if(s.count<1){
					alert('库存不足，请重新选择');
					return false;
				}
				console.log('规格参数已选完');
				alert('规格参数已选完');
				/*
					axios.get(`http://查询接口?action=querySkuPrice&skuId=`+this.result.skuid).then(
					       res =>{
							alert("价格接口查询结果："+JSON.stringify(res.data));
					       },
					       error =>{

					       }
					     )
					
					*/
			},
	toggleDialog(key, visible) {
	            this[key] = visible;
	}
    }
};
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style>
  .disabled {
            background-color: #ccc;
        }
  #sku {
    #display: flex;
	padding:20px;

  }
  .selected button{
            color: red;
        }
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.label-info {
  #padding: 5px;
  flex: 1;
  overflow: hidden;
  overflow-y: auto;
}
.label_img{
	width:45px;
}
a,
button {
  color: #4fc08d;
}

button {
  background: none;
  border: solid 1px;
  border-radius: 0.2em;
  font: inherit;
  margin:3px;
  padding: 0.5em 0.5em;
}

input{
	background: none;
	border: solid 1px;
	border-radius: 0.2em;
	font: inherit;
	margin:3px;
	padding: 0.5em 0.5em;
}
</style>