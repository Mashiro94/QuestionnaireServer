<template>
	<view>
		<cu-custom bgColor="bg-gradual-green" :isBack="false">
			<block slot="backText"></block>
			<block slot="content"></block>
		</cu-custom>
		<view class="text-center">
			<view style="margin: 100px 0px;"></view>
			<view class="solid-bottom text-xsl padding">
				<text class=" cuIcon-roundcheckfill text-green"></text>
			</view>
			<view class="solid-bottom text-xxl padding">
				<text class="text-black text-bold">您的问卷已提交成功！</text>
			</view>
		</view>
		<view class="padding flex flex-direction">
			<button class="cu-btn bg-gradual-blue lg" @click="handleClickResult()">查看结果</button>
		</view>
		<view class="padding flex flex-direction">
			<button class="cu-btn bg-gradual-green lg" @click="handleClickHome()">返回主页</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				questionnaireId: ''
			}
		},
		onLoad(option){
			this.$data.questionnaireId = option.questionnaireId;
		},
		methods: {
			handleClickHome: function() {
				uni.navigateTo({
					url: '/pages/index/index'
				})
			},
			handleClickResult: function() {
				uni.navigateTo({
					url: '/pages/questionnaire/result?questionnaireId=' + this.questionnaireId + '&userId=' + 1
				})
			}
		}
	}
</script>

<style>

</style>
