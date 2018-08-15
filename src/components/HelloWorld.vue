<style lang="less" scoped>
  * {
    margin: 0;
    padding: 0;
  }
  .tip-txt {
    color: #000;
    font-size: 18px;
    font-weight: bold;
    padding-left: 16px;
  }
  .btn-wrap {
    padding-top: 30px;
  }
  .submit-btn {
    color: #fff;
  }
</style>
<template>
  <div class="outside-wrap">
    <div>
      <som-form-group>
        <som-form-item label="手机号">
          <som-input placeholder="请输入11位的手机号" v-model="phone" name="phone"></som-input>
        </som-form-item>
        <som-form-item label="密码">
          <som-input placeholder="至少8位，同时包含数字和字母" type="password" v-model="firstPwd"></som-input>
        </som-form-item>
        <som-form-item label="确认密码">
          <som-input placeholder="再输一次" type="password" v-model="secondPwd"></som-input>
        </som-form-item>
        <div class="btn-wrap">
          <som-btn background="#FF571A" class="submit-btn" v-bury="'BURY_TEST_SUCCESS'" @click="handleSubmit">确认密码</som-btn>
        </div>
      </som-form-group>
    </div>
  </div>
</template>
<script>
import Vue from 'vue'
import { FormGroup, Toast } from 'som-ui'
import RegexpUtils from '@souche-f2e/souche-util/lib/regexp'
import HttpRequest from '@souche-f2e/http-request'
import VueBury from '@souche-vue/vue-burying'

Vue.use(VueBury, {
  env: 'development',
  baseOptions: {
    platform: 'ERP'
  }
})

export default {
  name: 'HelloWorld',
  data () {
    return {
      phone: '',
      firstPwd: '',
      secondPwd: ''
    }
  },
  methods: {
    handleSubmit () {
      let isPhoneCorrect = RegexpUtils.isPhone(this.phone)
      if (!isPhoneCorrect) {
        this.$toast('请输入11位的手机号码')
        return false
      }
      console.log(this.phone)
      this.$bury('BURY_TEST_SUCCESS')

      // 网络请求库前置钩子
      HttpRequest.beforeEach(function (data, next) {
        console.log('requestData', data)
        next()
      })

      HttpRequest.jsonp('https://git.souche-inc.com/souche-f2e/http-request', {
        pageSize: 20
      }).then(function (res) {
        console.log(res)
        this.$bury('BURY_TEST_SUCCESS')
        this.$toast('保存成功')
      }, function (err) {
        console.log(err)
      })
    }
  }
}

</script>
