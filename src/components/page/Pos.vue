<template>
  <div class="pos">
    <div class="pos">
      <el-row>
        <el-col :span="7" id="order-list">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border stripe show-summary style="width: 100%">
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="price" label="金额"></el-table-column>
                <el-table-column prop="count" label="数量"></el-table-column>
                <el-table-column label="操作">

                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="挂单"></el-tab-pane>
            <el-tab-pane label="外卖"></el-tab-pane>

            <div class="pos-button">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger">删除</el-button>
              <el-button type="success">结账</el-button>
            </div>
          </el-tabs>
        </el-col>

        <el-col :span="17" id="order-goods">
          <div class="oftenGoods">
            <div class="title">常用商品</div>
            <div class="oftenGoods-list">
              <ul class="oftenGoods-ul">
                <li @click="addOrderList(goods)" v-for="goods in oftenGoods">
                  <Goods :name="goods.goodsName" :price="goods.price">
                    <span>000</span>
                    <span slot="header">111</span>
                    <span>222</span>
                    <span>333</span>
                    <span>444</span>
                    <span slot="footer">555</span>
                  </Goods>


                  <Menus>
                    <template slot="bang" scope="biu">
                      {{biu.src}}
                    </template>
                  </Menus>


                </li>
              </ul>



            </div>
          </div>


          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class="cookList">
                  <li @click="addOrderList(goods)" v-for="goods in type0Goods">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食" ref="little_food">
                <ul class="cookList">
                  <li @click="addOrderList(goods)" v-for="goods in type1Goods">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class="cookList">
                  <li @click="addOrderList(goods)" v-for="goods in type2Goods">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class="cookList">
                  <li @click="addOrderList(goods)" v-for="goods in type3Goods">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">¥{{goods.price}}元</span>
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
  import axios from 'axios';
  import Goods from './Goods';
  import Menus from './Menus'

    export default {
      name: "Pos",
      components: {Goods,Menus},
      data(){
        return{
          tableData:[],
          oftenGoods:[],
          type0Goods:[],
          type1Goods:[],
          type2Goods:[],
          type3Goods:[],
          totalCount:0,
          totalMoney:0
        }
      },




      created(){
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
          .then(response =>{
            console.log(response);
            this.oftenGoods = response.data;
          })
          .catch(error =>{
            console.log(error);
            alert('网络错误，不能访问')
          });

        axios.get('http://jspang.com/DemoApi/typeGoods.php')
          .then(response =>{
            this.type0Goods = response.data[0];
            this.type1Goods = response.data[1];
            this.type2Goods = response.data[2];
            this.type3Goods = response.data[3];
          })
          .catch(error =>{
            console.log(error);
            alert('网络错误，不能访问')
          });
      },

      methods:{
        addOrderList(v){
          this.totalCount=0;
          this.totalMoney=0;

          console.log(this.$refs.little_food);

          let isHave=false;
          //判断这个商品是否存在列表中
          for(let i=0; i<this.tableData.length; i++){
            console.log(this.tableData[i].goodsName);
            if (this.tableData[i].goodsName == v.goodsName) {
              isHave=true;
            }
          }
          //存在数量增一
          if (isHave){
            let arr= this.tableData.filter(o => o.goodsName == v.goodsName)
            arr[0].count++;
          }
          else {
            //不存在推入表格
            let newGoods={goodsName:v.goodsName, price:v.price,count:1 }
            this.tableData.push(newGoods)
          }


          this.tableData.forEach((element)=>{
              this.totalCount+=element.count;
              this.totalMoney =this.totalMoney+(element.price*element.count);
            }

          )
        },

        delSingelGoods(g){
          console.log(g);
          this.tableData = this.tableData.filter(function (o) {
            console.log(o);
            return o.goodsName != g.goodsName;
          });
          let arr = [1, 2, 3, 4];
          console.log(arr.filter(o => o> 2));
          //this.tableData = this.tableData.filter(o => o.goodsName != g.goodsName);
        },

        delAllGoods(){
          this.tableData=[];
          this.totalCount=0;
          this.totalMoney=0;

        }

      }


    };
</script>

<style scoped>
  .pos-button{
    margin: 10px;
  }

  .pos-order{
    background-color: #f9fafc;
    border: 1px solid #c0ccda;
  }

  .title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
    text-align: left;
  }
  .oftenGoods-list ul li{
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
  .goods-type{
    clear: both;
  }
  .cookList li{
    list-style: none;
    width:23%;
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
  .oftenGoods-ul{
    float: left;
  }
</style>
<style>
  .el-tabs__header{
    margin: 0 15px 15px 15px;
  }
</style>
