<template>
  <div class="order">
    <van-nav-bar title="飞毛腿 外卖" >
      <template #left>
          <van-icon @click="toUserHandler" color="#333" name="manager" size="20" />
        </template>
    </van-nav-bar>
    <!-- 订单区域 -->
    <van-tabs v-model="active" color="#1659a0">
      <!-- 轮播图区域 -->
      <header class="header">
        <van-swipe class="my-swipe" :height="260" :autoplay="2000" indicator-color="white">
          <van-swipe-item >
            <img src="../../assets/1轮播.png" alt="">
          </van-swipe-item>
          <van-swipe-item>
            <img src="../../assets/2轮播.png" alt="">
          </van-swipe-item>
          <van-swipe-item>
            <img src="../../assets/3轮播.png" alt="">
          </van-swipe-item>
          <van-swipe-item>
            <img src="../../assets/4轮播.png" alt="">
          </van-swipe-item>
        </van-swipe>
      </header>
      <van-tab title="全部">
        <briup-order-item
          :data="orders" 
          @accept="acceptHandler"
          @reject="rejectHandler"
          @complete="completeHandler">
        </briup-order-item>
      </van-tab>
      <van-tab title="待接单" id="warnInfo1">
        <briup-order-item 
          :data="ordersStatusFilter('待接单')"
          @accept="acceptHandler"
          @reject="rejectHandler">
        </briup-order-item>
      </van-tab>
      <van-tab title="待服务" id="warnInfo2">
        <briup-order-item 
          :data="ordersStatusFilter('待服务')"
          @complete="completeHandler">
        </briup-order-item>
      </van-tab> 
      <van-tab title="待确认" id="warnInfo3">
        <briup-order-item 
          :data="ordersStatusFilter('待确认')"></briup-order-item>
      </van-tab>
      <van-tab title="已完成">
        <briup-order-item 
          :data="ordersStatusFilter('已完成')">
        </briup-order-item>
      </van-tab>
    </van-tabs>
    <!-- {{info}} -->
    <!-- {{orders}} -->
  </div>
</template>
<script>
import {mapState,mapActions,mapGetters} from 'vuex'
export default {
  data(){
    return {
      active:0,
      param:{
        waiterId:this.waiterId
      }
    }
  },
  computed: {
    ...mapState('order',['orders']),
    ...mapState('user',['info']),
    ...mapGetters('order',['ordersStatusFilter'])
  },
  created() {
    
  },
  mounted() {
    this.FindWaiterOrders(this.param)
    // console.log(this.ordersStatusFilter('待服务').length)
    // if(this.ordersStatusFilter('待接单').length = 0){
    //   let warnInfo = document.getElementById('warnInfo1')
    //   warnInfo.innerHTML = '暂无 待接订单~'
    //   warnInfo.style.marginTop = '5em'
    //   warnInfo.style.marginLeft = '10em'
    //   warnInfo.style.paddingBottom = '5em'
    // } else {
    //   let warnInfo = document.getElementById('warnInfo1')
    //   // warnInfo.innerHTML = ''
    //   warnInfo.style.marginTop = '0'
    //   warnInfo.style.marginLeft = '0'
    //   warnInfo.style.paddingBottom = '0'
    // }
    // if(this.ordersStatusFilter('待服务').length == 0){
    //   let warnInfo = document.getElementById('warnInfo2')
    //   warnInfo.innerHTML = '暂无 待配送订单~'
    //   warnInfo.style.marginTop = '5em'
    //   warnInfo.style.marginLeft = '10em'
    //   warnInfo.style.paddingBottom = '5em'
    // } else {
    //   let warnInfo = document.getElementById('warnInfo2')
    //   // warnInfo.innerHTML = ''
    //   warnInfo.style.marginTop = '0'
    //   warnInfo.style.marginLeft = '0'
    //   warnInfo.style.paddingBottom = '0'
    // }
    // if(this.ordersStatusFilter('待确认').length = 0){
    //   let warnInfo = document.getElementById('warnInfo3')
    //   warnInfo.innerHTML = '暂无 待确认订单~'
    //   warnInfo.style.marginTop = '5em'
    //   warnInfo.style.marginLeft = '10em'
    //   warnInfo.style.paddingBottom = '5em'
    // }
  },
  methods: {
    ...mapActions('order',['FindWaiterOrders','acceptOrder','rejectOrder','completeOrder']),
    toUserHandler(){
      this.$router.push({path:'./user'})
    },
    // 接受订单
    acceptHandler(orderId){
      this.params = {
        orderId:orderId,
        waiterId:this.info.id
      }
      // console.log(this.params)
       this.acceptOrder(this.params)
      .then((response)=>{
        this.$toast(response.statusText);
      })
    },
    // 拒绝订单
    rejectHandler(orderId){
      this.rejectOrder(orderId)
      .then((response)=>{
        this.$toast(response.statusText);
      })
    },
    // 完成
    completeHandler(orderId){
      this.completeOrder(orderId)
      .then((response)=>{
        this.$toast(response.statusText);
        this.FindWaiterOrders(this.param)
      })
    }
  }
}
</script>
<style scoped>
.order {
  background: #f1f1f1;
}
/* 轮播图 */
.header {
  height: 90px;
  width:94%;
  margin: .5em auto;
  overflow: hidden;
  border-radius: 8px;
}
.header img {
  width: 100%;
}
</style>