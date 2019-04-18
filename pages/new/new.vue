<template>
	<view class="content">
		<view class="text">
			请输入文章标题：<input v-model="biaoti" placeholder="标题">
			<hr>
			请输入正文内容：<input v-model="neirong" placeholder="正门内容">
			<hr>
			<view class="logo">
				<button type="button" class="btn-floating orange-gradient" @click="commit">
					<i class="yes">确认</i>
				</button>
			</view>
        </view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		onLoad() {

		},
		methods: {
			commit: function() {
				alert('成功');
			}
		}
	}
</script>

<style>
	.content {
		justify-content: center;
		text-align: center;
		height: 400upx;
	}
    .logo{
        height: 200upx;
        width: 200upx;
		margin-top: 20upx;
		margin: 0 auto;
		left: 0;
		right: 0;
    }
	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
	.text{
		display: flex;
		flex-direction: column;
	}
	.logo{
		display: flex;
		justify-content: center;
		width: 100%;
	}
	/* 圆形按钮 */
	.btn-floating{
		/* 阴影效果，四个参数分别是：水平阴影位置、垂直阴影位置、阴影尺寸位置大小、阴影颜色 */
		box-shadow: 2px 5px 10px #AAA;
		width: 600upx;
		height: 30px;
		margin: 10px;
		padding: 0;
		cursor: pointer;
		border: none;
	}
	/* 橘色渐变色按钮 */
	.orange-gradient{
		background: linear-gradient(40deg, #ffd86f, #fc6262);
		color: #FFF;
	}
	.yes{
		font-size: 30upx;
	}
</style>

