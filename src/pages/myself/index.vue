<template>
  <div class="counter-warp">
    <div class="myself-heard">
      <div class="head-portrait"><img :src="avatarUrl" class="avatar"></div>
      <div class="user-mag">
        <div class="username">{{userName}}</div>
        <div class="usertel">℡：{{userTel}}</div>
      </div>
    </div>
    <div>
      <div class="order-history">历史订单</div>
      <ul class="order-list clearfix">
        <li v-for="(item,index) in list" :key="index"><div class="cTime">{{item.shortDate}}订单<div style="float: right">＞</div></div>
          <div v-if="item.status === 0" class="state">未支付</div>
          <div v-if="item.status === 1" class="state">已支付</div>
          <div v-if="item.status === 2" class="state">过期未支付</div>
        </li>
      </ul>
    </div>
    <ul class="nav">
      <li  @click="goOrder">
        <img src="../../images/theorder.png"/><a>点餐</a>
      </li>
      <li @click="indent">
        <img src="../../images/indent.png" class="indent"/><a  >订单</a>
      </li>
      <li>
        <img src="../../images/themyself.png" class="myself"/><a style="color: #a4110f;">我的</a>
      </li>
    </ul>
  </div>
</template>

<script>
const APPLET_URL = require('./../../../static/js/address')
export default {
  data () {
    return {
      cTime: '2019.01.03',
      state: '订单已完成',
      userMsg: {},
      list: [],
      userName: '',
      userTel: '',
      avatarUrl: ''
    }
  },
  mounted () {
    let _that = this
    wx.login({
      success: function (res) {
        wx.request({
          url: APPLET_URL.url + 'http://192.168.188.19:8085/WX/getOpenId',
          data: {
            code: res.code
          },
          method: 'GET',
          success: function (res) {
            wx.request({
              url: 'http://192.168.188.19:8085/user/login/openId',
              data: {
                openId: res.data.openid
              },
              method: 'GET',
              success: function (res) {
                if (res.data) {
                  wx.request({
                    url: 'http://192.168.188.19:8085/user/userId?r=' + Math.random(), // 仅为示例，并非真实的接口地址
                    data: {
                      userId: res.data.data.id,
                      pageNum: '1',
                      pageSize: '10'
                    },
                    method: 'GET',
                    success (res) {
                      if (res.data) {
                        _that.list = res.data.data.list
                      }
                    }
                  })
                }
              }
            })
          }
        })
      }
    })
    wx.login({
      success: function (res) {
        wx.request({
          url: 'http://192.168.188.19:8085/WX/getOpenId',
          data: {
            code: res.code
          },
          method: 'GET',
          success: function (res) {
            console.log(res.data)
            wx.request({
              url: 'http://192.168.188.19:8085/user/login/openId',
              data: {
                openId: res.data.openid
              },
              method: 'GET',
              success: function (res) {
                if (res.data) {
                  console.log(res.data)
                  console.log(_that.userName)
                  _that.userName = res.data.data.nickName
                  _that.userTel = res.data.data.tel
                  _that.avatarUrl = res.data.data.avatarUrl
                }
              }
            })
          }
        })
      }
    })
  },
  components: {
  },
  methods: {
    goOrder () {
      wx.redirectTo({
        url: '/pages/chef/main?'
      })
    },
    indent () {
      wx.redirectTo({
        url: '/pages/chefIndent/main?'
      })
    }
  }
}
</script>
