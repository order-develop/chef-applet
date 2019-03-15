<template>
  <div class="counter-warp">
    <div class="header-img">
      <img src="../../images/header.png"/>
    </div>
    <div class="ordermsg">{{timeMsg}}</div>
    <div class="ordermsg">{{finish}}</div>
    <div class="order">
      <button @click="goPay">支付</button>
    </div>
    <ul class="nav">
      <li  @click="goOrder">
        <img src="../../images/theorder.png"/><a >点餐</a>
      </li>
      <li>
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
      finish: '未支付'
    }
  },

  components: {
    card
  },
  mounted () {
    let _that = this
    wx.request({
      url: 'http://192.168.188.19:8085/chef/food?r=' + Math.random(),
      data: {},
      method: 'GET',
      success (res) {
        if (res.data) {
          _that.theList = res.data.data
          _that.univalence = res.data.packagePrice
          _that.MoneyTotal = res.data.packagePrice
        }
      }
    })
  },
  methods: {
    goPay () {
      wx.redirectTo({
        url: '/pages/ConfirmPayment/main?'
      })
    },
    myself () {
      wx.redirectTo({
        url: '/pages/myself/main?'
      })
    },
    goOrder () {
      wx.redirectTo({
        url: '/pages/counter/main?'
      })
    }
  }
}
</script>
