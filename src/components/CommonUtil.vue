<template>
  <div class="btn-wrap">
    <som-btn @click="handleShare" size="small">点我分享</som-btn>
    <som-btn @click="handleGetPosition" size="small">点我获取位置</som-btn>
    <br><br><br><br><br>
    <div v-for="n in 20" :key="n">
      <oss-image src="http://img.souche.com/dccde1d428a14343c9e007970b3e37cb.jpg" :action="action">
      </oss-image>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import OSSImage from '@souche-ui/oss-image'
import VueShare from '@souche-vue/vue-share'
import VuePosition from '@souche-vue/vue-position'
import Tower from '@souche-f2e/tower'

Vue.use(OSSImage.component.vue)
Vue.use(VuePosition, {
  http: true
})

Vue.use(VueShare, {
  weixin: true,
  app: {
    Tower
  }
})
export default {
  name: 'CommonUtil',
  data () {
    return {
      action: {
        action: 'resize',
        w: 500
      }
    }
  },
  methods: {
    handleShare () {
      this.$share({
        title: 'title',
        content: 'content',
        url: location.href,
        image: 'https://s.gravatar.com/avatar/88ce765f15ea75a9e4d25fb8f4fb8c11?s=50&d=retro'
      })
      console.log('share', VueShare)
    },
    handleGetPosition () {
      VuePosition.getPosition().then(data => {
        console.log('当前所在位置：', data)
      }).catch(err => {
        console.log('error', err)
      })
    }
  }
}
</script>
