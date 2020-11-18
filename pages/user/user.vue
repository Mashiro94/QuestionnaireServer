<template>
	<cu-custom bgColor="bg-cyan" :isBack="false">
		<block slot="backText"></block>
		<block slot="content"></block>
	</cu-custom>
	
</template>

<script>
    export default {
		onReady:function(){
			uni.login({
			  success (res) {
			    if (res.code) {
			      //发起网络请求
			      wx.request({
			        url: 'http://localhost:8081/user/insertOne',
			        data: {
			          code: res.code,
			        },
					success: () => {
						console.log('user created');
					},
					fail: () => {
						console.log('user exists');
					}
			      })
			    } else {
			      console.log('登录失败！' + res.errMsg)
			    }
			  }
			})
		}
    }
	
</script>

<style>

</style>
