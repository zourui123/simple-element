<template>
  <div class="pos">
  	 <div>
  	 	<el-row>
  	 		<el-col :span="8" id="list_menu">
  	 			<el-tabs>
  	 				<el-tab-pane label="点餐">
  	 					<el-table :data="tableData" border   style="width:100%">
  	 						<el-table-column prop="goodsName" label="商品"></el-table-column>
  	 						<el-table-column prop="count" label="数量" width="60"></el-table-column>
  	 						<el-table-column prop="price" label="金额" width="60"></el-table-column>
  	 						<el-table-column  fixed="right" label="操作" width="100">
  	 							<template slot-scope="scope">
  	 								  <el-button type="text" size="small" @click="reduceProduct(scope.row)">删除</el-button>
                      <el-button type="text" size="small" @click="addProduct(scope.row)">增加</el-button>
  	 							</template>
  	 						</el-table-column>
  	 					</el-table>
                <div>总数量:{{allCount}} 总钱数:{{allMoney}}</div>
                <div class="btns">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger">删除</el-button>
                    <el-button type="success"  @click="checkout">结账</el-button>
                </div>
  	 				</el-tab-pane>
  	 				<el-tab-pane label="接单">
  	 					2
  	 				</el-tab-pane>
  	 				<el-tab-pane label="外卖">
  	 					3
  	 				</el-tab-pane>
  	 			</el-tabs>
  	 		</el-col>
  	 		<el-col :span="16" id="product_menu">
  
          <div class="often-goods">
              <div class="title">常用商品</div>
              <div class="often-goods-list">
                  <ul>
                      <li v-for="(item,index) in oftenGoods" :key="item.id" @click="addProduct(item)">
                          <span v-html="item.goodsName"></span>
                          <span class="o-price" v-html="item.price"></span>
                      </li>
                  </ul>
              </div>
          </div>
          <div class="goods-type">
              <el-tabs @tab-click="handleClick">
                  <el-tab-pane label="汉堡">
                      <ul class='cookList'>
                         <li v-for="goods in typeGoods">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
                      <el-tab-pane label="小食">
                       <ul class='cookList'>
                         <li v-for="(goods,index) in typeGoods">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
                  <el-tab-pane label="饮料">
                       <ul class='cookList'>
                         <li v-for="goods in typeGoods">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
                  <el-tab-pane label="套餐">
                       <ul class='cookList'>
                         <li v-for="goods in typeGoods">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                      </ul>
                  </el-tab-pane>
              </el-tabs>
          </div>
          
  	 		</el-col>
  	 	</el-row>
  	 </div>
  </div>
</template>

<script>
import { basePath } from '../service/api'
export default {
  name: 'Pos',
  data () {
    return {
    	   tableData: [],
         oftenGoods:[],
         typeGoods:[],
         allGoods:[],
         allMoney:0,
         allCount:0
    }
  },
  methods:{
     // 结账清算
     checkout(){
       if(this.allCount!=0){
          this.tableData = [];
          this.allCount = 0;
          this.allMoney = 0;
          this.$message({
            message: '结账成功',
            type: 'success'
          })
       }else{
         this.$message.error('不能空结账');
       }
     },
     handleClick(tab,event){
        let index = tab.index
        this.typeGoods = this.allGoods[index]
     },
     //复用的函数一定要提取出来
     getAllmoney(){
        this.allCount = 0;
        this.allMoney = 0;
        if(this.tableData){
          this.tableData.forEach( element => {
          console.log(element.count)
          this.allCount += element.count
          this.allMoney += element.count * element.price
        })
        }
     },
     reduceProduct(goods){
        this.tableData.splice(goods,1)
        this.getAllmoney()
     },
     addProduct(goods){
         // 第一次是空的 直接push 
         // if(this.tableData.length == 0){
         //    select.count = 1;
         //   this.tableData.push(select)
         // }else{
          // 如有有id 数量+1 没有直接push
               let  isHave = false
            for(var i=0;i<this.tableData.length;i++){
               console.log(this.tableData[i])
               if(goods.goodsId == this.tableData[i].goodsId){
                   isHave = true 
               }
            }
             if(isHave){
                 let arr = this.tableData.filter( o => o.goodsId == goods.goodsId)
                  arr[0].count++
                 // console.log(this.tableData)
               }else{
                  let newGoods = {
                    goodsId: goods.goodsId,
                    goodsName: goods.goodsName,
                    price: goods.price,
                    count:1 
                  }
                  this.tableData.push(newGoods)
              }
          this.getAllmoney()
      }
  },
  mounted(){
      // 上列表
      this.$https.get(basePath + 'oftenGoods.php')
      .then( res => {
          this.oftenGoods = res.data
      })
      .catch( error=>{
        console.log(error)
      })
      // 下列表
       this.$https.get(basePath + 'typeGoods.php')
      .then( res => {
        this.allGoods =  res.data
        this.typeGoods = res.data[0]
      })
      .catch( error=>{
        console.log(error)
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
#list_menu{
	height:100vh;
}
 .title{
       height: 20px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
   }
  .o-price{
      color:#58B7FF; 
   }
   .often-goods-list{
     height: 200px;
   }
   .cookList li{
       list-style: none;
       width:15%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
</style>
