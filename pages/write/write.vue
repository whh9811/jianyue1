<template>
	<view class="container">
		<!-- 文章标题输入框，和title变量绑定 -->
		<input type="text" v-model="title" placeholder="请输入标题" />
		<!-- 选择图片的按钮，点击触发chooseImg方法 -->
		<button class="add-btn" @tap="chooseImg">+图片</button>
		<!-- 文章内容输入的多行文本域，绑定content变量 -->
		<textarea placeholder="输入内容" v-model="content" class="content" />
		<text>预览</text>
		<!-- 使用graceUI的富文本解析功能，来预览文章内容 -->
		<view class="grace-text">、
				<rich-text :nodes="content" bindtap="tap"></rich-text>
		</view>
		<button class="green-btn" @tap="postArticle">发布文章</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Spring Boot文件上传示例!'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
	}
    .logo{
        height: 200upx;
        width: 200upx;
        margin-top: 200upx;
    }
	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
</style>

1/*