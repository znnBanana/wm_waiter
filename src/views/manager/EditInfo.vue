<template>
  <div class="home">
    <!-- 顶部导航栏 -->
    <div class="title">
      <!-- <van-row>
        <van-col span="4">
          <van-icon size="30" @click="CloseHome" style="padding:16px" name="cross" color="#333333"/>
        </van-col>
        <van-col :offset="2" span="18" style="font-size:20px;color:#333333;padding:16px;text-align:left">飞毛腿外卖服务平台</van-col>
      </van-row> -->
      <van-nav-bar
        title="我的"
        left-text="返回"
        left-arrow
        @click-left="onClickLeft"
        />
    </div>
    <!-- /顶部导航栏 -->
    
    <!-- 员工信息 -->
    <van-row>
      <van-col span="12">
        <van-cell title="请选择心仪头像" size="large" />
      </van-col>
      <van-col span="12">
        <van-uploader v-model="fileList" max-count="1" :after-read="afterRead" />
      </van-col>
    </van-row>
    <van-cell-group>
      <van-field
        v-model="waiterInfo.username"
        label="会员名"
        left-icon="smile-o"
        placeholder="请输入会员名"
      />
      <van-field
        v-model="waiterInfo.realname"
        clearable
        label="真实姓名"
        left-icon="music-o"
        placeholder='请输入真实姓名'
      />
      <van-field
        v-model="waiterInfo.telephone"
        clearable
        label="联系方式"
        left-icon="music-o"
        placeholder="请输入联系方式"
      />
      <van-field
        v-model="waiterInfo.idCard"
        clearable
        label="身份证号"
        left-icon="music-o"
        placeholder="请输入身份证号"
      />
      <van-field
        v-model="waiterInfo.bankCard"
        clearable
        label="银行卡号"
        left-icon="music-o"
        placeholder="请输入银行卡号"
      />
    </van-cell-group>
    <van-cell-group>
      <van-row>
        <van-col :span="12">
          <van-field
            v-model="waiterInfo.province"
            clearable
            label="地址"
            left-icon="music-o"
            placeholder="省份"
          />
        </van-col>
        <van-col :span="6">
          <van-field
            v-model="waiterInfo.city"
            clearable
            placeholder="城市"
          />
        </van-col>
        <van-col :span="6">
          <van-field
            v-model="waiterInfo.area"
            clearable
            placeholder="区域"
          />
        </van-col>
      </van-row>
    </van-cell-group>
    <!-- /员工信息 -->
    <div class="btn" @click="SaveWaiterHandler">
        修改
    </div>
    <!-- {{earn}} -->
    <!-- {{waiterInfo}} -->
  </div>
</template>

<script>
import {mapActions,mapGetters,mapState} from 'vuex'
export default {
  data() {
    return {
      fileList: [],
      photo: []
    }
  },
  created() {
    this.EarningWaiter(this.info.id)
    // 员工详细信息
    // console.log(this.info.id)
    this.WaiterById(this.info.id)
  },
  computed: {
    ...mapState('user',['info','earn','waiterInfo']),
    ...mapGetters('user',['IncomeTotal'])
  },
  methods: {
    ...mapActions('user',['EarningWaiter','WaiterById','UpdateWaiter']),
    afterRead(file) {
            //构造一个 FormData，把后台需要发送的参数添加
            this.formData = new FormData(); 
            //接口需要传的参数
            this.photo.push(file)
            this.photo.map((item,index)=>{
                if(index == 0){
                    this.formData.append('file1',item.file)
                }
            })
            console.log(this.formData.getAll('file1'),'file1')
            // console.log(this.formData.getAll('file2'),'file2')
        },
    // 回到我的页面
    onClickLeft(){
        this.$router.push({path:'./user'})
    },
    // 修改员工信息
    SaveWaiterHandler(){
      this.params = {
        id:this.info.id,
        type:'waiter',
        imgPhoto: 'http://134.175.100.63:8686/group1/M00/00/03/rBAABV62HeyAAOAUAABbY8DUoYs369.jpg',
        password:this.waiterInfo.password,
        money:this.waiterInfo.money,
        username:this.waiterInfo.username,
        realname:this.waiterInfo.realname,
        telephone:this.waiterInfo.telephone,
        idCard:this.waiterInfo.idCard,
        bankCard:this.waiterInfo.bankCard,
        province:this.waiterInfo.province,
        city:this.waiterInfo.city,
        area:this.waiterInfo.area,
      }
      // console.log(this.params)
      this.UpdateWaiter(this.params)
      .then(res=>{
        // console.log(this.info.id)
        this.WaiterById(this.info.id)
        this.$router.push({path:'./user'})
      })
    },
    // 返回到登录页面
    CloseHome(){
      this.$router.push({path:'/login'})
    }
  }
}
</script>

<style scoped>
.btn {
  background-color: #ededed;
  width: 90%;
  margin: 2em auto;
  line-height: 3em;
  text-align: center;
  border: 1px solid #ededed;
  border-radius: 1.5em;
  font-size: 14px;
}
.income_one{
  font-size: 18px;
}
.income{
  background: orange;
  padding: 1em;
  text-align: center;
  color: #fff;
  border: 1px solid #fff;
}
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>