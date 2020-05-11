<template>
  <div class="user">
    <!-- 标题 -->
    <div class="title">
      <!-- <van-nav-bar title="我的" /> -->
      <van-nav-bar title="个人中心">
        <template #left>
          <van-icon @click="returnHandler" name="arrow-left" color="#333" size="18" />
        </template>
        <template #right>
          <van-icon @click="logoutHandler" name="wap-home" color="#333" size="18" />
        </template>
      </van-nav-bar>
    </div>
    <!-- /标题 -->
    <!-- 头像信息 -->
    <!-- 用户信息 -->
    <div class="header">
      <van-row>
        <van-col :offset="2" :span="6">
          <div class="photo">
            <img :src="waiterInfo.imgPhoto" alt=""/>
          </div>
        </van-col>
        <van-col :span="16">
          <!-- {{waiterInfo}} -->
          <div class="name" @click="toEditWaiterInfoHandler">{{info.name}}</div>
          <!-- <div >{{waiterInfo.realname}}</div> -->
          <div style="color:#bbb">{{waiterInfo.realname}}，再忙也要记得吃饭呦~</div>
        </van-col>
      </van-row>
    </div>
    <!-- /用户信息 -->
    <!-- /头像信息 -->

    <!-- 余额 -->
    <div class="card_money" style="float:left" @click="toIncomeHandler">
      <div style="font-size:24px;">总收入</div>
      <div style="color:red;font-size:16px">￥{{IncomeTotal}}</div>
    </div>
    <div class="card_money recharge" style="float:right">
      <div style="font-size:24px" @click="WithdrawHandler()">可提现金额</div>
      <div style="color:#bbb;font-size:16px"> <span style="color:red">￥{{waiterInfo.money}}</span></div>
    </div>
    <div class="card_money recharge" style="float:left">
      <div style="font-size:24px" @click="realnameHandler()"><van-icon name="https://b.yzcdn.cn/vant/icon-demo-1126.png" /></div>
      <div style="font-size:16px;color:#108ee7">实名认证 </div>
    </div>
    <!-- /余额 -->

    <!-- 实名认证 -->
    <!-- <van-cell icon="manager-o" title="实名认证" is-link to="realname" /> -->
    <!-- /实名认证 -->

    <!-- 我的收入 -->
    <van-cell icon="gold-coin-o" title="我的收入" :value="IncomeTotal" is-link to="income_list" />
    <!-- /我的收入 -->

    <!-- 联系我们 -->
    <van-cell icon="phone-o" title="联系客服" @click="show = true" />
    <van-overlay :show="show" @click="show = false"  >
      <div class="wrapper" @click.stop>
        <div class="block">
          <van-cell title="联系客服" size="large" >
          <van-icon
            @click="closeModal()"
            slot="right-icon"
            name="cross"
            style="line-height: inherit;" />
          </van-cell>
          <van-cell title="0471-1234-888" />
        </div>
      </div>
    </van-overlay>
    <!-- /联系我们 -->

    <!-- 我的订单 -->
    <!-- <van-cell icon="orders-o" title="我的订单" is-link to="myorder" /> -->
    <!-- /我的订单 -->

    <div class="btn" @click="logoutHandler">
      退出
    </div>
    <!-- {{waiterInfo}} -->
  </div>
</template>
<script>
import {mapState, mapActions,mapGetters} from 'vuex'
export default {
  data() {
    return {
      show: false
    }
  },
  created() {
    this.EarningWaiter(this.info.id)
    // 员工详细信息
    this.WaiterById(this.info.id)
  },
  methods:{
    ...mapActions('user',['logout','EarningWaiter','WaiterById']),
    // 回到首页
    returnHandler(){
      this.$router.push({path:'./order'})
    },
    // 修改员工信息
    toEditWaiterInfoHandler(){
      // alert(1)
      this.$router.push({path:'./edit_info'})
    },
    // 实名认证
    realnameHandler(){
      this.$router.push({path:'./realname'})
    },
    // 收益列表
    toIncomeHandler(){
      this.$router.push({path:'./income_list'})
    },
    // 关闭模态框
    closeModal(){
      this.show = false
    },
    // 提现
    WithdrawHandler(){
      this.$router.push({path:'./withdraw'})
    },
    // 退出登录
    logoutHandler(){
      this.logout()
      .then(()=>{
        this.$router.push({path:'/login'})
      })
    }
  },
  computed:{
    ...mapState('user',['info','earn','waiterInfo']),
    ...mapGetters('user',['IncomeTotal'])
  }
}
</script>

<style scoped>
.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
.block {
  width: 240px;
  height: 120px;
  background-color: #fff;
}
.recharge {
  float: right;
}
.card_money div {
  text-align: center;
}
.card_money {
  padding: .5em;
  width: 36%;
  border: 1px solid #f1f1f1;
  border-radius: 5px;
  box-shadow: 3px 3px 3px #bbb;
  margin: 1em;
  margin-bottom: 2em;
}
.header {
  padding-top: 40px;
  text-align: center;
  /* background: #1659a0; */
  margin-bottom: 2em;
}

.header .photo {
  margin: 0 auto;
  width: 8em;
  height: 8em;
  border-radius: 50%;
  box-sizing: border-box;
  border:1px solid #bbb;
  overflow:hidden;
  /* padding: 1em; */
}
.header .name {
  line-height: 3em;
  font-size: 28px;
  /* color: #ffffff; */

}
.header .photo img {
 width: 100%;
 border-radius: 50%;
}
.btn {
  background-color: #108ee9;
  width: 90%;
  margin: 2em auto;
  /* line-height: 3em; */
  text-align: center;
  /* border: 1px solid #ededsed; */
  padding: .5em;
  border-radius: 1.5em;
  font-size: 16px;
  font-weight: bold;
  color: #f1f1f1;
}


</style>