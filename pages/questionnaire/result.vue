<template>
	<view>
		<cu-custom bgColor="bg-gradual-green" :isBack="true">
			<block slot="backText"></block>
			<block slot="content"></block>
		</cu-custom>
		<scroll-view scroll-x class="bg-gradual-green nav text-center">
			<view class="cu-item" :class="index==TabCur?'text-white cur':''" v-for="(item,index) in 3" :key="index" @tap="tabSelect" :data-id="index">
				{{ items[index] }}
			</view>
		</scroll-view>
		<view class="cu-card article" v-if="TabCur===0">
			<view class="cu-item shadow" v-for="result in results">
				<view class="cu-list menu-avatar">
					<view class="title">
						<view class="text-cut">{{ result.resultQuestionContent }}</view>
					</view>
					<view class="content text-gray" v-for="content in result.resultOptionContent">{{ content }}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				TabCur: 0, //答案0,图1,表2
				items:[
					'答案',
					'图',
					'表'
				],
				questionnaireId: '',
				results: []
			}
		},
		onLoad(option) {
			this.$data.questionnaireId = option.questionnaireId;
		},
		onReady: function() {
			uni.request({
				url: 'http://localhost:8081/result/selectByQuestionnaireUserId?questionnaireId=' + this.questionnaireId +
					'&userId=' + 1,
				success: (res) => {
					this.results = res.data;
				},
				fail: () => {
					uni.showToast({
						title: '资源加载错误,请重试',
						icon: 'none',
						duration: 2000
					})
				}
			})
		},
		methods: {
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			}
		}
	}
</script>

<style>

</style>
