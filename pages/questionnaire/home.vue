<template name="questionnaire">
	<view>
		<scroll-view scroll-y class="page">
			<cu-custom bgColor="bg-gradual-green" :isBack="false">
				<block slot="backText"></block>
				<block slot="content"></block>
			</cu-custom>
			<view class="cu-bar bg-white">
				<view class="content text-bold">选择一张问卷以开始</view>
			</view>
			<swiper class="card-swiper" :class="dotStyle?'square-dot':'round-dot'" :indicator-dots="true" :circular="true"
			 :autoplay="true" interval="5000" duration="500" @change="cardSwiper" indicator-color="#8799a3"
			 indicator-active-color="#0081ff">
				<swiper-item v-for="(item,index) in swiperList" :key="index" :class="cardCur==index?'cur':''">
					<view class="swiper-item">
						<image :src="item.url" mode="aspectFill" v-if="item.type=='image'"></image>
						<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
					</view>
				</swiper-item>
			</swiper>
			<view class="nav-list" style="margin-top: 20px;">
				<navigator hover-class="none" :url="'/pages/questionnaire/questions?questionnaireId=' + encodeURIComponent(JSON.stringify(item.questionnaireId))"
				 class="nav-li" navigateTo :class="'bg-green'" :style="[{animation: 'show ' + ((index+1)*0.2+1) + 's 1'}]" v-for="(item,index) in questionnaires"
				 :key="index">
					<view>{{item.questionnaireName}}</view>
					<view>{{item.questionnaireDescription}}</view>
				</navigator>
			</view>
			<view class="cu-tabbar-height"></view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		name: "basics",
		data() {
			return {
				questionnaires: [],
				swiperList: [{
					id: 0,
					type: 'image',
					url: 'http://img1.bj.wezhan.cn/content/sitefiles/2017149/images/7302974_1_f2d9cf40-e998-4521-b64b-77212b7490d2_resize_picture.png'
				}, {
					id: 1,
					type: 'image',
					url: 'http://img1.bj.wezhan.cn/content/sitefiles/2017149/images/11482695_7302975_2_8dd60cc4-7094-4979-ad92-61a732771d93_resize_picture%E6%8B%B7%E8%B4%9D_e108809f-2796-4119-85ad-c9883a1dbc88_resize_picture.jpeg',
				}, {
					id: 2,
					type: 'image',
					url: 'http://img1.bj.wezhan.cn/content/sitefiles/2017149/images/12167905_Screenshot_1_734a9ca4-1781-419a-886a-f374aa496488_resize_picture.jpeg'
				}]
			};
		},
		onReady: function() {
			uni.request({
				url: 'http://localhost:8081/questionnaire/selectAll',
				success: (res) => {
					this.$data.questionnaires = res.data;
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
			// cardSwiper
			cardSwiper(e) {
				this.cardCur = e.detail.current
			}
		},
		onShow() {}
	}
</script>

<style>
	.page {
		height: 100vh;
	}
</style>