<template>
  <div class="user">
    <!-- 标题 -->
    <div class="title">
      <!-- <van-nav-bar title="我的" /> -->
      <van-nav-bar title="我的">
        <template #right>
          <van-icon @click="logoutHandler" name="wap-home-o" size="18" />
        </template>
      </van-nav-bar>
    </div>
    <!-- /标题 -->
    <!-- 头像信息 -->
    <!-- 用户信息 -->
    <div class="header">
      <van-row>
        <van-col :span="16">
          <div class="name" @click="toEditWaiterInfoHandler">{{info.name}}</div>
          <div style="color:#bbb">再忙，也要记得吃饭呦~</div>
        </van-col>
        <van-col :span="8">
          <div class="photo">
            <img :src="waiterInfo.imgPhoto" alt=""/>
          </div>
        </van-col>
      </van-row>
    </div>
    <!-- /用户信息 -->
    <!-- /头像信息 -->

    <!-- 余额 -->
    <div class="card_money" style="float:left">
      <div style="font-size:24px;">收益金额</div>
      <div style="color:red;font-size:16px">￥{{IncomeTotal}}</div>
    </div>
    <div class="card_money recharge">
      <div style="font-size:24px" @click="WithdrawHandler()">提现</div>
      <div style="color:#bbb;font-size:16px">账户余额 <span style="color:red">￥{{waiterInfo.money}}</span></div>
    </div>
    <!-- /余额 -->

    <!-- 实名认证 -->
    <van-cell icon="manager-o" title="实名认证" is-link to="realname" />
    <!-- /实名认证 -->

    <!-- 我的收入 -->
    <van-cell icon="gold-coin-o" title="我的收入" :value="IncomeTotal" is-link to="income_list" />
    <!-- /我的收入 -->

    <!-- 我的订单 -->
    <!-- <van-cell icon="orders-o" title="我的订单" is-link to="myorder" /> -->
    <!-- /我的订单 -->

    <!-- <div class="btn" @click="logoutHandler">
      退出
    </div> -->
    <!-- {{waiterInfo}} -->
  </div>
</template>
<script>
import {mapState, mapActions,mapGetters} from 'vuex'
export default {
  data() {
    return {
      
    }
  },
  created() {
    this.EarningWaiter(this.info.id)
    // 员工详细信息
    this.WaiterById(this.info.id)
  },
  methods:{
    ...mapActions('user',['logout','EarningWaiter','WaiterById']),
    // 修改员工信息
    toEditWaiterInfoHandler(){
      // alert(1)
      this.$router.push({path:'./edit_info'})
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