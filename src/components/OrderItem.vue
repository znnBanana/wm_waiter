<template>
  <div class="order_item" style="background:#f1f1f1">
    <div v-if="data.length > 0">
      <van-panel :title="o.customer.username" :status="o.status" v-for="o in data" :key="o.id">
        <!-- {{o}} -->
        <van-row>
          <van-col span="4">
            <div class="order_img"><img src="../assets/wm_logo.jpeg" alt=""></div>
          </van-col>
          <van-col span="20">
            <van-row style="margin-top:.8em">
              <van-col :span="4" style="text-align:center"><van-icon name="orders-o" /></van-col>
              <van-col :span="20">
                订单名称：{{o.orderLines[0].product.name}}
              </van-col>
            </van-row>
            <van-row>
              <van-col :span="4" style="text-align:center"><van-icon name="balance-o" /></van-col>
              <van-col :span="10">总价： {{o.total}}</van-col>
            </van-row>
          
            <van-row>
              <van-col :span="4" style="text-align:center"><van-icon name="clock-o" /></van-col>
              <van-col :span="20">下单时间：{{o.orderTime | datefmt}}</van-col>
            </van-row>
            <!-- <van-row>
              <van-col :span="4" style="text-align:center"><van-icon name="description" /></van-col>
              <van-col :span="20">
                服务内容：{{o.orderLines[0].product.description}}
              </van-col>
            </van-row> -->
            <van-row style="margin-bottom:.8em">
              <van-col :span="4" style="text-align:center"><van-icon name="location-o" /></van-col>
              <van-col :span="20">送单地址： {{o.address.province+" "+o.address.city+" "+o.address.area}}</van-col>
            </van-row>
          </van-col>
        </van-row>
        <div slot="footer" style="text-align:right">
          <div v-if="o.status === '待接单'">
            <van-button size="small" type="warning" plain @click="rejectHandler(o.id)">拒绝</van-button>&nbsp;
            <van-button size="small" type="primary" plain @click="acceptHandler(o.id)">接受</van-button> 
          </div>
          <div v-else-if="o.status === '待服务'">
            <van-button size="small" type="warning" plain @click="serviceCompleteHandler(o.id)">完成</van-button>&nbsp;
          </div>
        </div>
      </van-panel>
    </div>
    
  </div>
</template>
<script>
export default {
  props:["data"],
  methods:{
    rejectHandler(orderId){
      this.$emit('reject',orderId);
    },
    acceptHandler(orderId){
      this.$emit('accept',orderId);
    },
    serviceCompleteHandler(orderId){
      this.$emit('complete',orderId);
    }
    
  }
}
</script>

<style scoped>
.order_img img {
  width: 45px;
  height: 45px;
  margin: 1em 0 0 1em;
}
</style>