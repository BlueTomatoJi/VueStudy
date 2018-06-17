<template>

  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-wodezichan"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>

</template>

<script>
    export default {
        name: "LeftNav"
    }
</script>

<style scoped>
  .left-nav{
    width: 5%;
    background-color: #1d8ce0;
    color: #fff;
    font-size: 10px;
    float: left;
    height: 100%;
  }
  .iconfont{
    font-size: 24px;
  }
  .left-nav ul{
    padding: 0;
    margin: 0;
  }
  .left-nav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
  }
</style>
