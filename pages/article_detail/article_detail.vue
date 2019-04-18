<template>
	<view class="container">
		<text class="article-title">{{ article.title }}</text>
		<view class="article-info">
			<image :src="article.avatar" class="avatar small"></image>
			<view class="title1">
				<text class="info-text">{{ article.nickname }}</text>
				<text class="info-text">{{ handleTime(article.createTime) }}</text>
			</view>
			<!-- 登录用户和文章作者不是同一个人，就显示关注或取消关注按钮 -->
			<button v-if="userId != article.uId && !followed" class="btn-floating orange-gradient" @tap="follow">关注</button>
			<button v-if="userId != article.uId && followed" class="btn-floating orange-gradient" @tap="cancelFollow">取消</button>
		</view>

		<view class="grace-text" style="margin-top: 10px;"><rich-text :nodes="article.content" bindtap="tap"></rich-text></view>
		<text class="info-text">评论{{ comments.length }}</text>
		<view class="comment-item" v-for="(comment, index) in comments" :key="index">
			<view class="left"><image :src="comment.avatar" class="avatar small"></image></view>
			<view class="right">
				<text>{{ comment.nickname }}</text>
				<text>{{ comment.content }}</text>
				<view>
					<text style="margin-right: 10px;">{{ comments.length - index }}楼</text>
					<text>{{ handleTime(comment.commentTime) }}</text>
				</view>
			</view>
		</view>
		<input class="uni-input comment-box" type="text" placeholder="写下你的评论" v-model="content" required="required" />
		<button class="btn-floating1 greed-gradient " @tap="send">提交</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			article: {
				aId: 0,
				uId: 0,
				title: '',
				content: '',
				avatar: '',
				nickname: '',
				createTime: ''
			},
			comments: [],
			content: '',
			userId: uni.getStorageSync('login_key').userId,
			followed: false
		};
	},
	onLoad: function(option) {
		//option为object类型，会序列化上个页面传递的参数
		this.article.aId = option.aId;
	},
	onShow: function() {
		this.getArticle();
	},
	onPullDownRefresh: function() {
		this.getArticle();
	},
	methods: {
		getArticle: function() {
			var _this = this;
			uni.request({
				url: this.apiServer + '/article/' + this.article.aId,
				method: 'GET',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					userId: this.userId
				},
				success: res => {
					// console.log(res.data.data.article);
					_this.article.aId = res.data.data.article.id;
					_this.article.uId = res.data.data.article.uId;
					_this.article.title = res.data.data.article.title;
					_this.article.content = res.data.data.article.content;
					_this.article.nickname = res.data.data.article.nickname;
					_this.article.avatar = res.data.data.article.avatar;
					_this.article.createTime = res.data.data.article.createTime;
					_this.comments = res.data.data.comments;
					if (res.data.data.followed === '已关注') {
						_this.followed = true;
					}
				},
				complete: function() {
					uni.stopPullDownRefresh();
				}
			});
		},
		handleTime: function(date) {
			var d = new Date(date);
			var year = d.getFullYear();
			var month = d.getMonth() + 1;
			var day = d.getDate() < 10 ? '0' + d.getDate() : '' + d.getDate();
			var hour = d.getHours() < 10 ? '0' + d.getHours() : '' + d.getHours();
			var minutes = d.getMinutes() < 10 ? '0' + d.getMinutes() : '' + d.getMinutes();
			var seconds = d.getSeconds() < 10 ? '0' + d.getSeconds() : '' + d.getSeconds();
			return year + '-' + month + '-' + day + ' ' + hour + ':' + minutes + ':' + seconds;
		},
		send: function() {
			console.log('评论人编号：' + this.userId + ',文章编号：' + this.article.aId + '，评论内容：' + this.content);
			uni.request({
				url: this.apiServer + '/comment/add',
				method: 'POST',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					aId: this.article.aId,
					uId: this.userId,
					content: this.content
				},
				success: res => {
					if (res.data.code === 0) {
						uni.showToast({
							title: '评论成功'
						});
						this.getArticle();
						this.content = '';
					}
				}
			});
		},
		follow:function(){
			uni.request({
				url: this.apiServer + '/follow/add',
				method: 'POST',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					fromUId: this.userId,
					toUId: this.article.uId
				},
				success: res => {
					if (res.data.code === 0) {
						uni.showToast({
							title: '关注成功'
						});
						this.followed = true;
					}
				}
			});
		},
		cancelFollow:function(){
			uni.request({
				url: this.apiServer + '/follow/cancel',
				method: 'POST',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					fromUId: this.userId,
					toUId: this.article.uId
				},
				success: res => {
					if (res.data.code === 0) {
						uni.showToast({
							title: '已取消关注'
						});
						this.followed = false;
					}
				}
			});
		}
	}
};
</script>

<style>
.content {
	margin-bottom: 10px;
	margin-top: 10px;
	padding: 5px;
	border-bottom: 1px solid #eee;
}
.img-list {
	display: flex;
	flex-direction: column;
}
.img-item {
	width: 100%;
	height: 150px;
	margin-bottom: 5px;
}
.img-item image {
	width: 50upx;
	height: 50upx;
	border-radius: 5px;
}
.comment-item {
	display: flex;
	align-items: center;
	border-bottom: 1px solid #eee;
	margin-bottom: 10px;
	padding: 5px;
}
.comment-item .left {
	flex: 1 1 15%;
}
.comment-item .right {
	flex: 1 1 85%;
	display: flex;
	flex-direction: column;
}
.comment-box {
	border: 1px solid #fff;
	border-radius: 5px;
	background-color: #eee;
}
.uni-input{
	height: 80upx;
}
.follow-btn {
	height: 33px;
	width: 80px;
	font-size: 12pt;
	text-align: center;
	padding-bottom: 20px;
	margin-right: 0px;
}
.cancel{
	background-color: #aaa;
}
.article-title{
	font-weight: 900;
}
.article-info{
	display: flex;
	align-items: center;
	margin-top: 20upx;
}
.article-info image{
	width: 100upx;
	height: 100upx;
}
.info-text{
	margin-left: 10upx;
	margin-right: 10upx;
	font-size: 35upx;
	color: #999999;
}
.grace-text{
	font-size: 35upx;
	color: #292929;
	margin-bottom: 10upx;
}
.title1{
	display: flex;
	flex-direction: column;
}
.btn-floating{
		/* 阴影效果，四个参数分别是：水平阴影位置、垂直阴影位置、阴影尺寸位置大小、阴影颜色 */
		box-shadow: 2px 5px 10px #AAA;
		height: 30px;
		width: 60px;
		font-size: 32upx;
		text-align: center;
		padding-bottom: 20px;
		margin-right: 0px;
		padding: 0;
		cursor: pointer;
		border: none;
	}
	/* 橘色渐变色按钮 */
	.orange-gradient{
		background: linear-gradient(40deg, #ffd86f, #fc6262);
		color: #FFF;
	}
	.left image{
		width: 100upx;
		height: 100upx;
	}
	.right{
		font-size: 35upx;
		color: #292929;
	}
	.btn-floating1{
		/* 阴影效果，四个参数分别是：水平阴影位置、垂直阴影位置、阴影尺寸位置大小、阴影颜色 */
		box-shadow: 2px 5px 10px #AAA;
		width: 92%;
		height: 40px;
		margin: 10px;
		padding: 0;
		cursor: pointer;
		border: none;
	}
	/* 橘色渐变色按钮 */
	.greed-gradient{
		background: linear-gradient(40deg, #AFEEEE, #008080);
		color: #FFF;
	}
</style>
