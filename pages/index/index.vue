<template>
	<view style="overflow: hidden;">
		<view class="title">
			<scroll-view class="tab-title " scroll-x="true" id="tab-title">
				<view v-for="(tab, index) in tabs" :key="index" :class="[tabCurrentIndex == index ? 'tab-current' : 'tabpage']" :id="'tabTag-' + index"
				 @tap="tabChange">
					{{ tab.name }}
				</view>
			</scroll-view>
		</view>
		<view class="hr1"></view>
		<swiper class="tab-swiper-full" :current="swiperCurrentIndex" @change="swiperChange" :style="{ height: tabHeight + 'px' }">
			<swiper-item key="0">
				<view data-scindex="0">
					<!-- <view class="content">
						<view class="Bcard2" v-for="(card, index) in cards " :key= "index">	
							<view class="Scard1">
								<p class="acticle">{{card.acticle}}</p>
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
						</view>
					</view> -->
					<view class="conotainer">
						
						<view class="article-box">
							<view class="article" v-for="(article,index) in articles" :key="index">
								<!-- 标题 -->
								<text class="article-title" @tap="gotoDetail(article.id)">{{article.title}}</text>
								<!-- 大于等于三张图片的显示方式 -->
								<view class="images" v-if="article.imgs.length >= 3">
									<view class="thumbnail-box">
										<view class="thumbnail-item" v-for="(item,index) in article.imgs" :key="index" v-if="index < 3">
											<image :src="item.imgs"></image>
										</view>
										
									</view>
								</view>
								<!-- 小于三张图片的显示方式 -->
								<view class="images" v-else-if="article.imgs.length >= 1">
									<view class="text-img-box1">
										<view class="left">
											<text class="text1">{{handleContent(article.content)}}...</text>
										</view>
										<view class="right">
											<image :src="article.imgs[article.imgs.length -1].imgs"></image>
										</view>
									</view>
								</view>	
								<!-- 没有图片的显示方式 -->
								<view class="text-box" v-else>
									<text class="text1">{{handleContent(article.content)}}...</text>
								</view>
								<!-- 文章作者等信息 -->
								<view class="article-info">
									<image :src="article.avatar" class="avatar small"></image>
									<text class="info-text">{{article.nickname}}</text>
									<text class="info-text">{{handleTime(article.createTime)}}</text>
								</view>
							</view>
						</view>
					</view>
				</view>
			</swiper-item>
			<swiper-item key="1">
				<view data-scindex="1">
					<view class="content">
						<image src="../../static/background1.png" class="pg" />
					</view>
				</view>
			</swiper-item>
			<swiper-item key="2">
				<view data-scindex="2">
					<view class="content">
						<navigator url="../my/my" hover-class="navigator-hover">
							<button class="circle-btn1"><text class="icon-text">点此进入我的主页</text></button>
						</navigator>
					</view>
				</view>
			</swiper-item>
		</swiper>
		<navigator url="../new/new" hover-class="navigator-hover">
			<button class="circle-btn"><text class="icon-text">+</text></button>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabCurrentIndex: 0,
				swiperCurrentIndex: 0,
				titleShowId: 'tabTag-0',
				tabHeight: 300,
				articles: [],
				tabs: [
					//标签名称 , 分类 id , 加载更多, 加载的页码
					{
						name: '文章',
						id: 'pwd1',
						loadingType: 0,
						page: 1
					},
					{
						name: '图片',
						id: 'pwd2',
						loadingType: 0,
						page: 1
					},
					{
						name: '个人',
						id: 'pwd3',
						loadingType: 0,
						page: 1
					}
				],
// 				cards: [{
// 						acticle: 'Spring Boot文件上传示例',
// 						text: '一、创建upload模块项目 可按照下图顺序进行创建：创建一个新的模块springboot 接下来的创建方法跟之前的创建方法都一样，不过要注意这...',
// 						eye: 20,
// 						i: 5,
// 						love: 50,
// 						time: '03.25 17:27'
// 					},
// 					{
// 						acticle: '第四周前后端分离开发笔记',
// 						text: '学习笔记： SPA：单页应用 场景 技术栈 ...',
// 						eye: 53,
// 						i: 2,
// 						love: 42,
// 						time: '03.22 14:27'
// 					},
// 					{
// 						acticle: 'SpringBoot和Mybatis的集成练习',
// 						text: '初步可了解一下SpringBoot思维导图进行初步学习了解，接下来我们进行起步练习。如何学习SpringBoot？ 我的第一个Spring Bo...',
// 						eye: 10,
// 						i: 7,
// 						love: 30,
// 						time: '03.19 22:28'
// 					},
// 					{
// 						acticle: '各种软件版本的含义',
// 						text: 'RC版本 RC：(Release Candidate)Candidate是候选人的意思，用在软件上就是候选版本。Release是发行、发布的意思...',
// 						eye: 18,
// 						i: 6,
// 						love: 94,
// 						time: '03.19 22:07'
// 					},
// 				],
				showKeyboard: false
			}
		},
		onLoad: function() {
			this.getArticles();
		},
		onShow: function() {},
		onPullDownRefresh: function() {
			this.getArticles();
		},
		methods: {
			getArticles: function() {
				var _this = this;
				uni.request({
					url: this.apiServer + '/article/list',
					method: 'GET',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						_this.articles = res.data.data;
					},
					complete: function() {
						uni.stopPullDownRefresh();
					}
				});
			},
			gotoDetail: function(aId) {
				uni.navigateTo({
					url: '../article_detail/article_detail?aId=' + aId
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
			handleContent: function(content) {
				content = content.replace(/(\n)/g, '');
				content = content.replace(/(\t)/g, '');
				content = content.replace(/(\r)/g, '');
				content = content.replace(/<\/?[^>]*>/g, '');
				content = content.replace(/\s*/g, '');
				return content.substring(0, 50);
			},
			done(password) {
				console.log(password);
				console.log(this.$refs);
			},
			confirmPwd() {
				this.$refs.keyboard.show();
			},
			onInput(e) {
				//e.cancel = true;
			},
			onConfirm(e) {
				let password = e.value;
			},
			tabChange(e) {
				var index = e.target.id.replace('tabTag-', '');
				this.swiperCurrentIndex = index;
				this.tabCurrentIndex = index;
				this.titleShowId = 'tabTag-' + index;
			},
			swiperChange: function(e) {
				var index = e.detail.current;
				this.tabCurrentIndex = index;
				this.titleShowId = 'tabTag-' + index;
			}
		},
		onReady() {
			//获取屏幕高度及比例
			var winInfo = uni.getSystemInfo({
				success: res => {
					var windowHeight = res.windowHeight;
					//获取头部标题高度
					var dom = uni.createSelectorQuery().select('#tab-title');
					dom.fields({
						size: true
					}, res2 => {
						if (!res2) {
							return;
						}
						//计算得出滚动区域的高度
						this.tabHeight = windowHeight - res2.height;
						console.log(this.tabHeight);
					}).exec();
				}
			});
		}
	}
</script>
<style>
	.content {
		width: 90%;
		margin: 0 auto;
		flex: 1 1 auto;
	}

	.circle-btn {
		position: absolute;
		bottom: 20px;
		right: 10px;
		width: 40px;
		height: 40px;
		border-radius: 50%;
		background-color: #de533a;
		background: linear-gradient(40deg, #AFEEEE, #008080);
		/* background-image: url(../../static/Pencil32.png); */
		box-shadow: 2px 5px 10px #aaa;
		cursor: pointer;
		border: none;
		outline: none;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #FFFFFF;
	}

	.circle-btn1 {
		width: 100%;
		height: 40px;
		border-radius: 5%;
		background-color: #de533a;
		background: linear-gradient(40deg, #008080F, #008080);
		/* background-image: url(../../static/Pencil32.png); */
		box-shadow: 2px 5px 10px #aaa;
		cursor: pointer;
		border: none;
		outline: none;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #FFFFFF;
	}

	.title {
		display: flex;
	}

	.hr1 {
		width: 100%;
		height: 0.5px;
		margin-top: 5upx;
		background: #bfbfbf;
	}

	.Bcard1 {
		display: flex;
		margin-top: 70upx;
		text-align: center;
	}

	.Scard {
		width: 300px;
		height: 50px;
		border-right: 1px solid #EEEEEE;
	}

	.Scard1 {
		margin-top: 15upx;
		width: 100%%;
		border-bottom: 1px solid #bfbfbf;
	}

	.acticle {
		margin-bottom: 20upx;
	}

	.text {
		margin-bottom: 20upx;
		font-size: 30upx;
		color: #999999;
	}

	.icon {
		display: flex;
		align-items: center;
	}

	.icon-small {
		width: 35upx;
		height: 35upx;
		margin-right: 10upx;
	}

	.icon-size {
		margin-right: 30upx;
		margin-bottom: 10upx;
		font-size: 30upx;
		color: #999999;
	}

	.pg {
		width: 100%;
		height: 1000upx;
	}
	.article{
		display: flex;
		flex-direction: column;
		border-bottom: 1upx solid #EEEEEE;
		margin-top: 15px;
		padding-bottom: 10px;
	}
	.avatar-box{
		display: flex;
		flex-direction: row;
	}
	.article-title{
		font-weight: 900;
		margin-bottom: 10px;
	}
	.text-img-box{
		display: flex;
		flex-direction: row;
	}
	.text-img-box1{
		display: flex;
		justify-content: space-between;
	}
	.thumbnail-box{
		display: flex;
		flex-direction: row;
		margin-left: 30upx;
	}
	.thumbnail-item image{
		width: 220upx;
		height: 220upx;
	}
	.left{
		flex: 1 1 60%;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 4;
		overflow: hidden;
	}
	.right{
		
	}
	.right image{
		width: 220upx;
		height: 220upx;
	}
	.article-info{
		display:flex;
		align-items: center;
		color: grey;
		width: 100%;
		margin-top: 10px;
	}
	.avatar{
		width: 75upx;
		height: 75upx;
		border-radius: 50%;
	}
	.info-text{
		margin-left: 10px;
		font-size: 34upx;
	}
	.article-box{
		margin-left: 15upx;
	}
	.text1{
		color: #616161;
		font-size: 35upx;
	}
	.images{
		margin-left: 20upx;
		margin-right: 20upx;
	}
</style>
