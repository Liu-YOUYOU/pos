<template>
    <div class="pos">
      <el-row>
        <el-col :span="7" class="pos-order" id="order-list">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border  show-summar style="width: 100%;">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="count" label="量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="delSingleGood(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="count-total">
                <span><small>数量：</small>{{ totalCount }}</span><span><small>金额：</small>{{ totalPrice }}元</span>
              </div>
              <div class="ctro-btn">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAllGoods">删除</el-button>
                <el-button type="success" @click="checkOut">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">挂单</el-tab-pane>
            <el-tab-pane label="外卖">外卖</el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="goods-list">
              <ul>
                <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                  <span>{{ goods.goodsName }}</span>
                  <span class="often-price">￥{{ goods.price }}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <div>
                  <ul class="foods-list">
                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                      <span class="foods-img"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foods-name">{{ goods.goodsName }}</span>
                      <span class="foods-price">￥{{ goods.price }}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <div>
                  <ul class="foods-list">
                    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                      <span class="foods-img"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foods-name">{{ goods.goodsName }}</span>
                      <span class="foods-price">￥{{ goods.price }}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <div>
                  <ul class="foods-list">
                    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                      <span class="foods-img"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foods-name">{{ goods.goodsName }}</span>
                      <span class="foods-price">￥{{ goods.price }}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <div>
                  <ul class="foods-list">
                    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                      <span class="foods-img"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foods-name">{{ goods.goodsName }}</span>
                      <span class="foods-price">￥{{ goods.price }}元</span>
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
<script type="text/ecmascript-6">
    import axios from 'axios';
    export default{
      name: 'pos',
      data() {
        return {
          tableData: [],
          oftenGoods: [],
          type0Goods:[],
          type1Goods:[],
          type2Goods:[],
          type3Goods:[],
          totalCount: 0,
          totalPrice: 0
        }
      },
      created: function () {
        //商品列表数据请求
        axios.get('http://shxccSer/DemoApi/oftenGoods.php')
          .then(response => {
//            常用商品
//            console.log(response);
            this.oftenGoods = response.data;
          })
          .catch(error => {
            console.log(error);
          })
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
          .then(response => {
//            商品分类
//            console.log(response);
            this.type0Goods = response.data[0];
            this.type1Goods = response.data[1];
            this.type2Goods = response.data[2];
            this.type3Goods = response.data[3];
          })
          .catch(error => {
            console.log(error);
          })
      },
      mounted: function () {
        //获取可视化高度
        var orderHeight = document.body.clientHeight;
        document.getElementById('order-list').style.height = orderHeight +'px';
      },
      methods: {
        addOrderList(goods){
          this.totalCount = 0;
          this.totalPrice = 0;
          //判断商品是否已存在订单列表中
          let isHave = false;
          for(let i = 0 ; i < this.tableData.length ; i ++){
            if(this.tableData[i].goodsId == goods.goodsId){
              isHave = true;
            }
          }
          //根据判断的值编写业务逻辑
          if(isHave) {
            //存在就进行数量添加
            let arr = this.tableData.filter(o => o.goodsId == goods.goodsId)
            console.log(arr);
            arr[0].count++;
          } else {
            //不存在就插入订单列表中
            let newGoods = {goodsId: goods.goodsId,goodsName: goods.goodsName,price: goods.price,count:1};
            this.tableData.push(newGoods);
          }
          //计算总价格、数量的说明
          this.getAllMoney();
        },
        //删除单个商品
        delSingleGood(goods){
          this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
          this.getAllMoney();
        },
        //删除所有商品
        delAllGoods(){
          this.totalPrice = 0;
          this.totalCount = 0;
          this.tableData = [];
        },
        //汇总数量和金额
        getAllMoney(){
          this.totalCount = 0;
          this.totalPrice = 0;
          if(this.tableData){
            this.tableData.forEach((element) => {
              this.totalCount += element.count;
              this.totalPrice = this.totalPrice + (element.count*element.price);
            })
          }
        },
        //模拟结账有好提示
        checkOut(){
          if(this.totalCount != 0){
            this.totalPrice = 0;
            this.totalCount = 0;
            this.tableData = [];
            this.$message({
              message: '结账成功，感谢你又为店里出了一份力!',
              type: 'success'
            });
          } else {
            this.$message.error('不能空结。老板了解你急切的心情！');
          }
        }
      }
    }
</script>
<style scoped>
  .pos-order{
    background: #F9FAFC;
    border-right:1px solid #C0CCDA;
    height: 100%;
  }
  .ctro-btn{
    margin-top: 10px;
  }
  .count-total{
    background: #FFF;
    padding: 10px;
    border-bottom: 1px solid #D3dec6;
  }
  .count-total span{
    display: inline-block;
    margin-right: 20px;
  }
  .title{
    height: 21px;
    border-bottom: 1px solid #D3dce6;
    background: #F9FAFC;
    padding: 10px;
    text-align: left;
  }
  .goods-list ul li{
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 10px;
    background: #FFF;
    cursor: pointer;
  }
  .often-price{
    color: #58B7FF;
  }
  .goods-type{
    clear: both;
  }
  .foods-list li{
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    float: left;
    margin: 2px;
    padding: 2px;
    background: #FFF;
    cursor: pointer;
  }
  .foods-list li span{
    float: left;
    display: block;
  }
  .foods-img{
    width: 40%;
    font-size: 0;
  }
  .foods-name{
    font-size: 16px;
    width: 40%;
    text-align: left;
    padding-left: 10px;
    color: brown;
  }
  .foods-price{
    font-size: 16px;
    padding: 10px 0 0 10px;
  }
</style>
