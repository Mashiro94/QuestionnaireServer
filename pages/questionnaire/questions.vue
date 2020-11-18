<template>
	<view>
		<cu-custom bgColor="bg-gradual-green" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">{{ questionnaire.questionnaireName }}</block>
		</cu-custom>
		<view class="solids-bottom padding-xs flex align-center view-text">
			<view class="flex-sub text-center">
				<view class="solid-bottom text-xxl padding">
					<text>{{ questionnaire.questionnaireName }}</text>
				</view>
				<view class="solid-bottom text-xl text-gray">
					<text>{{ questionnaire.questionnaireDescription }}</text>
				</view>
			</view>
		</view>

		<view v-for="question in questions" :key="question.questionId">
			<view class="cu-bar bg-white solid-bottom margin-top">
				<view class="action">
					<text class="cuIcon-title text-green"></text>{{ question.questionContent }}
				</view>
			</view>
			<radio-group class="block" @change="RadioChange" v-if="question.questionType=='单选'">
				<view class="cu-form-group" v-for="option in question.options" :key="option.optionId">
					<view class="title">{{ option.optionContent }}</view>
					<radio :class="radio=='option.optionValue'?'checked':''" :checked="radio=='option.optionValue'?true:false" 
						:value="question.questionId + '-' + option.optionValue"></radio>
				</view>
			</radio-group>
			<checkbox-group class="block" @change="CheckboxChange" v-if="question.questionType=='多选'">
				<view class="cu-form-group" v-for="option in question.options" :key="option.optionId">
					<view class="title">{{ option.optionContent }}</view>
					<checkbox :class="answers[optionId].checked?'checked':''" :checked="answers[optionId].checked?true:false"
						:value="question.questionId + '-' + option.optionValue"></checkbox>
				</view>
			</checkbox-group>
		</view>
		
		<view class="cu-bar bg-white btn-group foot">
			<button class="cu-btn bg-blue shadow-blur round" @click="handleClickResult()">查看结果</button>
			<button class="cu-btn bg-green shadow-blur round" @click="handleClickSubmit()">提交答案</button>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				questionnaireId: '',
				questionnaire: [],
				questions: [],
				answers: [],
				requests: []
			}
		},
		onLoad(option){
			this.$data.questionnaireId = option.questionnaireId;
		},
		onReady: function() {
			uni.request({
				url: 'http://localhost:8081/questionnaire/selectById/' + this.$data.questionnaireId,
				success: (res) => {
					this.$data.questionnaire = res.data;
					this.$data.questions = res.data.questions;
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
			RadioChange(e) {
				var str = e.detail.value;
				var split = str.search(/-/i);
				var id = str.substring(0, split);
				var chosen = str.substring(split + 1);
				this.$data.answers[Number(id)] = chosen;
			},
			CheckboxChange(e) {
				var items = e.detail.value;
				var result = '';
				for (var i = 0; i < items.length; ++i) {
					var str = items[i];
					var split = str.search(/-/i);
					var id = str.substring(0, split);
					var chosen = str.substring(split + 1);
					result += '-' + chosen;
				}
				this.$data.answers[Number(id)] = result;
			},
			handleClickSubmit: function() {
				//检查问卷是否填写完成
				var jsonLength = 0;
				for (var answer in this.$data.answers) jsonLength++;
				if (jsonLength != this.$data.questions.length) {
					uni.showToast({
						title: '尚未完成问卷',
						icon: 'none',
						duration: 2000
					})
				} else {
					//map通过指定函数调用一个数组中每一项元素，来创建一个新数组
					const requests = this.$data.questions.map(question => ({
						"resultQuestionId": question.questionId,
						"resultContent": this.$data.answers[question.questionId]
					}));
					uni.request({
						url: 'http://localhost:8081/result/insertOne/',
						data: requests,
						method: 'POST'
					})
					uni.navigateTo({
						url: '/pages/questionnaire/complete?questionnaireId=' + this.questionnaireId + '&userId=' + 1
					})
				}
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
