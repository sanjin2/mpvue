<template>
    <div class="container">
        <div class="userinfo">
            <img src="" alt="" />
        </div>
    <YearProgress></YearProgress>

    <button class="btn">添加图书</button>

        个人中心页面
        <button open-type="getUserInfo" lang="zh_CN" bindgetuserinfo="doLogin">获取用户信息</button>
    </div>
</template>
<script>
import qcloud from 'wafer2-client-sdk'
import config from '@/config'
export default {
methods: {
     getWxLogin: function ({encryptedData, iv, userinfo}) {
      const self = this
      wx.login({
        success: function (loginResult) {
          console.log('loginResult', loginResult)
          var loginParams = {
            code: loginResult.code,
            encryptedData: encryptedData,
            iv: iv
          }
          qcloud.setLoginUrl(config.loginUrl)
          qcloud.requestLogin({
            loginParams,
            success () {
                // 获取用户信息
              qcloud.request({
                url: config.userUrl,
                login: true,
                success (userRes) {
                  showSuccess('登录成功')
                  wx.setStorageSync('userinfo', userRes.data.data)
                  self.userinfo = userRes.data.data
                }
              })
            },
            fail (error) {
              showModal('登录失败', error)
            }
          })
        },
        fail: function (loginError) {
          showModal('登录失败', loginError)
        }
      })
    },

    login (e) {
      const self = this
      // 查看是否授权
      wx.getSetting({
        success: function (res) {
          // 授权信息里有用户信息
          if (res.authSetting['scope.userInfo']) {
            // 检查用户登录是否过期
            wx.checkSession({
              success: function () {
                // 没过期 直接成功
                showSuccess('登录成功')
              },
              fail: function () {
                // 过期了 重新登录 先清楚登录的状态
                qcloud.clearSession()
                // 登录态已过期，需重新登录
                // 登录需要的加密信息
                var options = {
                  encryptedData: e.mp.detail.encryptedData,
                  iv: e.mp.detail.iv,
                  userinfo: e.mp.detail.userInfo
                }
                self.getWxLogin(options)
              }

            })
          } else {
            showModal('用户未授权', e.mp.detail.errMsg)
          }
        }

      })



    },


    data(){
        return {
            userinfo:{}
        }
    },
    // create(){
// async  created(){
// doLogin: function  aa( ) {
//     const session = qcloud.Session.get()

//     if (session) {
//         // 第二次登录
//         // 或者本地已经有登录态
//         // 可使用本函数更新登录态
//         qcloud.loginWithCode({
//             success: res => {
//                 this.setData({ userInfo: res, logged: true })
//                 util.showSuccess('登录成功')
//             },
//             fail: err => {
//                 console.error(err)
//                 util.showModel('登录错误', err.message)
//             }
//         })
//     } else {
//         // 首次登录
//         qcloud.login({
//             success: res => {
//                 this.setData({ userInfo: res, logged: true })
//                 util.showSuccess('登录成功')
//             },
//             fail: err => {
//                 console.error(err)
//                 util.showModel('登录错误', err.message)
//             }
//         })
//     }
// }   
//     }
    }
}
// }
</script>
<style lang='scss'>
.container{
    padding: 0 30rpx;
    .userinfo{
        margin-top:100rpx; 
    }
}
</style>


