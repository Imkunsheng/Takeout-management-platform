<template>
<div class="pos">
    <el-row>
        <el-col :span="7" class="pos-order" id="order-list">
            <el-tabs>
                <el-tab-pane label="点餐">
                    <el-table :data="tableData" border style="width: 100%;">
                        <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                        <el-table-column prop="count" label="数量" width:100></el-table-column>
                        <el-table-column prop="price" label="金额"></el-table-column>
                        <el-table-column prop="control" label="操作"  fixed="right">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="delSinggleData(scope.row)">删除</el-button>
                              <!--神奇的地方是这里并没有传进来商品的tableData,竟然用scope.row就可以-->
                                <el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                  <div>
                    <span>数量：{{totalCount}}</span>
                    <span>金额：{{totalMoney}}</span>

                  </div>
                    <div class="btn">
                       <el-button type="warning" >挂单</el-button>
                       <el-button type="danger" @click="delAllData()">删除</el-button>
                       <el-button type="success" @click="checkout()">结账</el-button>

                    </div>
                </el-tab-pane>
                <el-tab-pane label="挂单">

                </el-tab-pane>
                <el-tab-pane label="外卖">

                </el-tab-pane>
            </el-tabs>
        </el-col>
        <el-col  span="17">
            <div>
                <div class="goods">常用商品</div>
                <div class="goodList">
                    <ul>
                        <li v-for="good in goods" @click="addOrderList(good)">
                            <span>{{good.goodsName}}</span>
                            <span class="oldPrice">￥{{good.price}}元</span>
                        </li>
                    </ul>
                </div>
            </div>

            <div class="good-type">
                <el-tabs>
                    <el-tab-pane label="汉堡">
                        <div>
                            <ul class='cookList'>
                                <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="零食">
                      <div>
                        <ul class='cookList'>
                          <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                        </ul>
                      </div>
                    </el-tab-pane>
                    <el-tab-pane label="饮料">
                      <div>
                        <ul class='cookList'>
                          <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                        </ul>
                      </div>
                    </el-tab-pane>
                    <el-tab-pane label="套餐">
                      <div>
                        <ul class='cookList'>
                          <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                          </li>
                        </ul>
                      </div>
                    </el-tab-pane>
                </el-tabs>
            </div>
        </el-col>
    </el-row>
</div>
</template>
<script>
import axios from 'axios'
export default {
    name: 'pos',
    data(){
       return{
           tableData: [
//                   注释这些是什么呢？其实是这些Jason不是写死在data里的，
//                   应该是用axios从后端拿的.
//           {
//               goodsName: '可口可乐',
//               price: 8,
//               count:1
//           },
//           {
//               goodsName: '香辣鸡腿堡',
//               price: 15,
//               count:1
//           },
//           {
//               goodsName: '爱心薯条',
//               price: 8,
//               count:1
//           },
//           {
//               goodsName: '甜筒',
//               price: 8,
//               count:1
//           }
           ],
           goods:[
//               {
//                   goodsId:1,
//                   goodsName:'香辣鸡腿堡',
//                   price:18
//               }, {
//                   goodsId:2,
//                   goodsName:'田园鸡腿堡',
//                   price:15
//               }, {
//                   goodsId:3,
//                   goodsName:'和风汉堡',
//                   price:15
//               }, {
//                   goodsId:4,
//                   goodsName:'快乐全家桶',
//                   price:80
//               }, {
//                   goodsId:5,
//                   goodsName:'脆皮炸鸡腿',
//                   price:10
//               }, {
//                   goodsId:6,
//                   goodsName:'魔法鸡块',
//                   price:20
//               }, {
//                   goodsId:7,
//                   goodsName:'可乐大杯',
//                   price:10
//               }, {
//                   goodsId:8,
//                   goodsName:'雪顶咖啡',
//                   price:18
//               }, {
//                   goodsId:9,
//                   goodsName:'大块鸡米花',
//                   price:15
//               }, {
//                   goodsId:20,
//                   goodsName:'香脆鸡柳',
//                   price:17
//               }
           ],
           type0Goods:[
//               {
//                   goodsId:1,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
//                   goodsName:'香辣鸡腿堡',
//                   price:18
//               }, {
//                   goodsId:2,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
//                   goodsName:'田园鸡腿堡',
//                   price:15
//               }, {
//                   goodsId:3,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
//                   goodsName:'和风汉堡',
//                   price:15
//               }, {
//                   goodsId:4,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
//                   goodsName:'快乐全家桶',
//                   price:80
//               }, {
//                   goodsId:5,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
//                   goodsName:'脆皮炸鸡腿',
//                   price:10
//               }, {
//                   goodsId:6,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
//                   goodsName:'魔法鸡块',
//                   price:20
//               }, {
//                   goodsId:7,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
//                   goodsName:'可乐大杯',
//                   price:10
//               }, {
//                   goodsId:8,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
//                   goodsName:'雪顶咖啡',
//                   price:18
//               }, {
//                   goodsId:9,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
//                   goodsName:'大块鸡米花',
//                   price:15
//               }, {
//                   goodsId:20,
//                   goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
//                   goodsName:'香脆鸡柳',
//                   price:17
//               }

           ],
           type1Goods:[],
           type2Goods:[],
           type3Goods:[],
           totalCount:0,
           totalMoney:0
       }
    },
    created(){
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
        .then(response=>{
            console.log(response);
            this.goods=response.data;
        })
    .catch(error=>{
            console.log(error);
            alert('网络错误，不能访问');
        })


      axios.get('http://jspang.com/DemoApi/typeGoods.php')
        .then(response=>{
        console.log(response);
      this.type0Goods=response.data[0];
      this.type1Goods=response.data[1];
      this.type2Goods=response.data[2];
      this.type3Goods=response.data[3];
    })
    .catch(error=>{
        console.log(error);
      alert('网络错误，不能访问');
    })
    },
    mounted:function () {
        var orderheight = document.body.clientHeight;
        document.getElementById('order-list').style.height = orderheight+'px'

    },
  methods:{

    //添加订单列表的方法
    addOrderList(goods){
      this.totalCount=0; //汇总数量清0
      this.totalMoney=0;
      let isHave=false;
      //判断是否这个商品已经存在于订单列表
      for (let i=0; i<this.tableData.length;i++){
        console.log(this.tableData[i].goodsId);
        if(this.tableData[i].goodsId==goods.goodsId){
          isHave=true; //存在
        }
      }
      //根据isHave的值判断订单列表中是否已经有此商品
      if(isHave){
        //存在就进行数量添加
        let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
        arr[0].count++;
        //console.log(arr);
      }else{
        //不存在就推入数组
        let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newGoods);
      }
      this.getAllmoney()
    },
//    删除某个商品
    delSinggleData(goods){
      this.totalCount=0
      this.totalMoney=0
      this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId)
      this.getAllmoney()
    },
//    删除所有商品
    delAllData(){
      this.tableData=[];
      this.tableData=0;
      this.totalMoney=0
    },
//    模拟结账
    checkout(){
      if(this.totalCount!=0){
        this.tableData=[];
        this.totalCount=0;
        this.totalMoney=0;
        this.$message({
          message:"结账成功，欢迎下次光临",
          type:'success'
        })
      }else {
        this.$message.error('亲，您还没点餐呢，我懂你一定是太急了')
      }
    },
    getAllmoney(){
      this.totalCount=0;
      this.totalMoney=0;
      if(this.tableData) {
        //进行数量和价格的汇总计算
        this.tableData.forEach((element) => {
        this.totalCount += element.count;
        this.totalMoney = this.totalMoney + (element.price * element.count);
      })
      }
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order{
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
    height: 100%;
}
    .btn{
        margin-top: 10px;
    }
.goods{
    height: 18px;
    border-bottom:1px solid #d3dce6;
    background-color: #F9FAFC;
    padding: 10px;
    text-align: left;
}
.goodList ul li {
    list-style:none;
    float: left;
    border: 1px solid #E5E8f2;
    background-color: #fff;
    padding: 10px;
    margin: 10px;
}
.oldPrice{
    color: #56b7ff;
}
.good-type{
    clear: both;
}
.cookList li{
    list-style: none;
    width:23%;
    border:1px solid #E5E9F2;
    height: 100%;
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
