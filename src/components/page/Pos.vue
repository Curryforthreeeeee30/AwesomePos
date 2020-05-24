<template>
  <div class="pos">
    <el-row>
      <el-col :span='6' class="pos-order" id="order-list">
        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
          <el-tab-pane label="点餐" name="first">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template slot-scope="scope"> 
                  <el-button type="text" size="small" @click="delsingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量：</small>{{totalCount}}   &nbsp;&nbsp;&nbsp;&nbsp;
              <small>金额：</small>{{totalMoney}}元
            </div>
            <div class="div-btn">
              <el-button :plain="true" type="warning">挂单</el-button>
              <el-button :plain="true" type="danger" @click="delallGoods">删除</el-button>
              <el-button :plain="true" type="success" @click="checkOut">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单" name="second">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖" name="third">
            外卖
          </el-tab-pane>
        </el-tabs>

      </el-col>
      <el-col :span='18'>
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs v-model="activeName_2" type="card" @tab-click="handleClick">
            <el-tab-pane label="主食" name="first">
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
            <el-tab-pane label="小食" name="second">
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
            <el-tab-pane label="饮料" name="third">
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
            <el-tab-pane label="套餐" name="forth">
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
import axios from "axios"
export default {
  name: 'pos',
  data(){
    return{
      tableData:[],
      activeName: 'first',
      activeName_2: 'first',
      // oftenGoods:[],
      // type0Goods:[],
      // type1Goods:[],
      // type2Goods:[],
      // type3Goods:[]
      oftenGoods:[
          {
              goodsId:1,
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsName:'和风汉堡',
              price:15
          }, {
              goodsId:4,
              goodsName:'快乐全家桶',
              price:80
          }, {
              goodsId:5,
              goodsName:'脆皮炸鸡腿',
              price:10
          }, {
              goodsId:6,
              goodsName:'魔法鸡块',
              price:20
          }, {
              goodsId:7,
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
              goodsName:'雪顶咖啡',
              price:18
          }, {
              goodsId:9,
              goodsName:'大块鸡米花',
              price:15
          }, {
              goodsId:20,
              goodsName:'香脆鸡柳',
              price:17
          }

      ],
      type0Goods:[{
              goodsId:1,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191444328139.png",
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191445238413.png",
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191445351148.png",
              goodsName:'和风汉堡',
              price:15
          }
      ],
      type1Goods:[
      {
              goodsId:4,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191446258631.png",
              goodsName:'快乐全家桶',
              price:80
          }, {
              goodsId:5,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191446371767.png",
              goodsName:'脆皮炸鸡腿',
              price:10
          }, {
              goodsId:6,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191431284513.png",
              goodsName:'魔法鸡块',
              price:20
          }
      ],
      type2Goods:[
      {
              goodsId:7,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2015/11/201511061810017423.png",
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/05/202005141153262808.png",
              goodsName:'雪顶咖啡',
              price:18
          }, {
              goodsId:9,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191433146156.png",
              goodsName:'大块鸡米花',
              price:15
          }
      ],
      type3Goods:[
      {
              goodsId:20,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2020/03/202003191433289748.png",
              goodsName:'香脆鸡柳',
              price:17
          },{
              goodsId:21,
              goodsImg:"http://officialwebsitestorage.blob.core.chinacloudapi.cn/public/upload/attachment/2019/08/201908131737148079.png",
              goodsName:'苹果片',
              price:17
          }

      ],
      totalMoney : 0,
      totalCount : 0
    }
  },
  // created(){
  //     axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
  //     .then(response=>{
  //       //  console.log(response);
  //        this.oftenGoods=response.data;
  //     })
  //     .catch(error=>{
  //       //  console.log(error);
  //         alert('网络错误，不能访问');
  //     }),
  //     //读取分类商品列表
  //     axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
  //     .then(response=>{
  //       //  console.log(response);
  //        //this.oftenGoods=response.data;
  //        this.type0Goods=response.data[0];
  //        this.type1Goods=response.data[1];
  //        this.type2Goods=response.data[2];
  //        this.type3Goods=response.data[3];

  //     })
  //     .catch(error=>{
  //         console.log(error);
  //         alert('网络错误，不能访问');
  //     })
  // },
  // mounted在所有dom加载完成之后再进行
  mounted:function(){
    var orderHeight = document.body.clientHeight;
    // console.log(orderHeight);
    document.getElementById('order-list').style.height = orderHeight+'px';
  },
  methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      },
      addOrderList(goods){

        let isHave = false;
        for(let i=0;i<this.tableData.length;i++){
          if(goods.goodsId==this.tableData[i].goodsId){
            isHave = true;
          }
        }
        // 根据isHave来编写业务逻辑
        if(isHave){
          let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
          arr[0].count++;
        }else{
          let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
          this.tableData.push(newGoods);
        }

        this.getAllMoney();
       
      },
       //汇总数量和金额
      getAllMoney(){
            this.totalCount=0;
            this.totalMoney=0;
            if(this.tableData){
                this.tableData.forEach((element) => {
                this.totalCount+=element.count;
                this.totalMoney=this.totalMoney+(element.price*element.count);   
            });
            }

      },
      delsingleGoods(goods){
          this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
          this.getAllMoney();
      },
      delallGoods(){
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
      },
      checkOut() {
            if (this.totalCount!=0) {
                this.tableData = [];
                this.totalCount = 0;
                this.totalMoney = 0;
                this.$message({
                    message: '结账成功，感谢你又为店里出了一份力!',
                    type: 'success'
                });
            }else{
              this.$message({
                    message: '不能空结。老板了解你急切的心情！',
                    type: 'warning'
                });
            }
        }

  }
}
</script>


<style scoped>
  .pos-order{
    background-color: #F9FAFC;
    border-right:1px solid #C0CCDA;
  }
  .div-btn{
    margin-top:10px;
  }
  .title{
    height:20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
    text-align: left;
  }
  .often-goods-list ul li{
    list-style: none;
    float:left;
    border:1px solid #E5E9F2;
    padding: 10px;
    margin: 10px;
    background-color: #FFF;
    cursor: pointer;
  }
  .o-price{
    color: #58B7FF;
  }
  .goods-type{
    clear:both;
  }
  .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor:pointer;

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
   .totalDiv {
    height: auot;
    overflow: hidden;
    text-align: right;
    font-size: 16px;
    background-color: #fff;
    border-bottom: 1px solid #E5E9F2;
    padding: 10px;
   }
</style>
