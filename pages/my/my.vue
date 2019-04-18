<template>
	<view class="container">
		<view class="top">
			<view class="avatar-box">
				<image
					src="../../static/default.png"
					mode="scaleToFill"
					class="avatar"
					v-if="!storageData.login">
					</image>
				<image
					:src="avatar"
					mode="scaleToFill"
					class="avatar"
					v-if="storageData.login"
				></image>
			</view>
			<view class="top1">
				<view class="info-box">
				<navigator url="../signin/signin" v-if="!storageData.login">点击登录</navigator>
				<text v-if="storageData.login">{{nickname}}</text>
				<navigator url="../setting/setting" v-if="storageData.login">
					<text class="shezhi">个人设置</text>
				</navigator>
				</view>
			</view>
			<navigator url="../zhuce/zhuce" hover-class="navigator-hover" v-if="!storageData.login" class="name">
				<text class="setting">还没注册，请点击！</text>
			</navigator>
		</view>
		<view class="Bcard1" v-if="storageData.login">
			<view class="Scard">
				<view class="num">{{wenzhang}}</view>
				<view class="word">文章</view>
			</view>
			<view class="Scard">
				<view class="num">{{guanzhu}}</view>
				<view class="word">关注</view>
			</view>
			<view class="Scard">
				<view class="num">{{xiaoxi}}</view>
				<view class="word">消息</view>
			</view>
			<view class="Scard">
				<view class="num">{{jifen}}</view>
				<view class="word">积分</view>
			</view>
		</view>
		<!-- <view class="Bcard2" v-if="storageData.login" v-for="(card, index) in cards " :key= "index">	
			<view class="Scard1">
				<navigator url="../write/write" hover-class="navigator-hover">
					<p class="acticle">{{card.acticle}}</p>
				</navigator>
				<p class="text">{{card.text}}</p>
				<view class="icon">
					<image src="../../static/eye.png" class="icon-small" />
					<p class="icon-size">{{card.eye}}</p>
					<image src="../../static/i.png" class="icon-small" />
					<p class="icon-size">{{card.i}}</p>
					<image src="../../static/love.png" class="icon-small" />
					<p class="icon-size">{{card.love}}</p>
					<p class="icon-size">{{card.time}}</p>
				</view>
			</view>
		</view> -->
	</view>
</template>

<script>
var loginRes, _self;
export default {
	data() {
		return {
			avatar: '',
			nickname: '',
			wenzhang:4,
			guanzhu:5,
			xiaoxi:66,
			jifen:100,
			storageData: {},
			cards: [
			    {
			        acticle: 'Spring Boot文件上传示例',
					text:'一、创建upload模块项目 可按照下图顺序进行创建：创建一个新的模块springboot 接下来的创建方法跟之前的创建方法都一样，不过要注意这...',
					eye:20,
					i:5,
					love:50,
					time:'03.25 17:27'
				},
			    {
			        acticle: '第四周前后端分离开发笔记',
					text:'学习笔记： SPA：单页应用 场景 技术栈 ...',
					eye:53,
					i:2,
					love:42,
					time:'03.22 14:27'
			    },
			    {
			        acticle: 'SpringBoot和Mybatis的集成练习',
					text:'初步可了解一下SpringBoot思维导图进行初步学习了解，接下来我们进行起步练习。如何学习SpringBoot？ 我的第一个Spring Bo...',
					eye:10,
					i:7,
					love:30,
					time:'03.19 22:28'
				},
			    {
			        acticle: '各种软件版本的含义',
					text:'RC版本 RC：(Release Candidate)Candidate是候选人的意思，用在软件上就是候选版本。Release是发行、发布的意思...',
					eye:18,
					i:6,
					love:94,
					time:'03.19 22:07'
			    },
			]
		};
	},
	onLoad: function() {},
	onShow: function() {
		var _this = this;
		const loginKey = uni.getStorageSync('login_key');
		if (loginKey) {
			// console.log(loginKey);
			this.storageData = {
				login: loginKey.login,
				nickname: loginKey.nickname,
				avatar: loginKey.avatar,
			};
		} else {
			this.storageData = {
				login: false
			};
		}
		uni.request({
			url: this.apiServer + '/user/' + uni.getStorageSync('login_key').userId,
			method: 'GET',
			header: { 'content-type': 'application/json' },
			success: res => {
				if (res.data.code === 0) {
					console.log(res.data.data.avatar+'————————————');
					console.log(res.data.data.nickname+'————————————');
					_this.avatar = res.data.data.avatar;
					_this.nickname = res.data.data.nickname;
				}
			}
		});
	},
	methods: {
	}
}
</script>

<style scoped>
.top {
	display: flex;
	flex-direction: column;
	height: 70px;
	margin-top: 10px;
	
}
.avatar-box{
	text-align: center;
	margin-bottom: 10upx;
}
.info-box{
	display: flex;
	margin: 0 auto;
	left: 0;
	right: 0;
	text-align: center;
}
.shezhi{
	margin-left: 40upx;
	color: #2FBE80;
	font-size: 32upx;
	font-weight: 600;
}
.num{
	font-size: 40upx;
	text-align: center;
}
.top1{
	display: flex;
	text-align: center;
}

.Bcard1{
	display: flex;
	margin-top: 70upx;
	text-align: center;
}
.Scard{
	width:300px;
	height:50px;
	border-right: 1px solid #EEEEEE;
}
.Scard1{
	margin-top: 15upx;
	width: 100%%;
	border-bottom: 1px solid #bfbfbf;
}
.acticle{
	margin-bottom: 20upx;
}
.text{
	margin-bottom: 20upx;
	font-size: 30upx;
	color: #999999;
}
.icon{
	display: flex;
	align-items: center;
}
.icon-small{
	width: 35upx;
	height: 35upx;
	margin-right: 10upx;
}
.icon-size{
	margin-right: 30upx;
	margin-bottom: 10upx;
	font-size: 30upx;
	color: #999999;
}
.setting{
	display: flex;
	justify-content: center;
	color: #008080;
	margin-top: 60upx;
}
</style>
