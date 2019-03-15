<template>
  <div class="counter-warp">
    <div class="header-img">
      <img src="../../images/header.png"/>
    </div>

    <div class="verification">
      <button @click="cancel" class="cancel">取消</button>
      <button @clcik="ok" class="ok">确认取餐</button>
    </div>

    <ul class="nav">
      <li @click="goOrder">
        <img src="../../images/theorder.png"/><a >点餐</a>
      </li>
      <li @clcik="indent">
        <img src="../../images/theindent.png" class="indent"/><a @click="indent" style="color: #a4110f;">订单</a>
      </li>
      <li @click="myself">
        <img src="../../images/myself.png" class="myself"/><a >我的</a>
      </li>
    </ul>
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
      timeMsg: '2019.01.23订单',
      finish: '订单已完成'
    }
  },
  onLoad (option) {
    let _that = this
    _that.orderId = option.orderId
  },
  components: {
    card
  },
  methods: {
    goOrder () {
      wx.redirectTo({
        url: '/pages/chef/main?'
      })
    },
    myself () {
      wx.redirectTo({
        url: '/pages/myself/main?'
      })
    },
    indent () {
      wx.redirectTo({
        url: '/pages/chefIndent/main?'
      })
    },
    cancel () {

    },
    ok () {
    }
  }
}
</script>
