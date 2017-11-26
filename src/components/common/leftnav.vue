<template>
    <div class="left-nav">
      <ul>
        <li>
          <i class="icon iconfont icon-wodezichan"></i>
          <div>收银</div>
        </li>
        <li>
          <i class="icon iconfont icon-31dianpu"></i>
          <div>店铺</div>
        </li>
        <li>
          <i class="icon iconfont icon-hanbao"></i>
          <div>商品</div>
        </li>
        <li>
          <i class="icon iconfont icon-31huiyuanqia"></i>
          <div>会员</div>
        </li>
        <li>
          <i class="icon iconfont icon-iconfontshujutongji"></i>
          <div>统计</div>
        </li>
        <li>
          <i class="icon iconfont icon-shezhi"></i>
          <div>设置</div>
        </li>
      </ul>
    </div>
</template>
<script type="text/ecmascript-6">
    export default{
        name: 'hello'
    }
</script>
<style>
  .left-nav{
    color:#FFF;
    font-size:10px;
    height: 100%;
    background:#1D8CE0;
    float: left;
    width: 5%;
  }
  .iconfont{
    font-size:24px;
  }
  .left-nav ul{
    margin: 0;
    padding: 0;
  }
  .left-nav li{
    list-style:none;
    text-align:center;
    border-bottom:1px solid #20a0ff;
    padding: 10px;
  }
</style>
