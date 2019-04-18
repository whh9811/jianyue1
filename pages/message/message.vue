<template>
	<view class="content">
		<view class="zimu">
			<p class="w">↑</p><p class="w">☆</p><p class="w">A</p><p class="w">B</p><p class="w">C</p><p class="w">D</p>
			<p class="w">E</p><p class="w">F</p><p class="w">G</p><p class="w">H</p><p class="w">I</p><p class="w">J</p>
			<p class="w">K</p><p class="w">L</p><p class="w">M</p><p class="w">N</p><p class="w">O</p><p class="w">P</p>
			<p class="w">Q</p><p class="w">R</p><p class="w">S</p><p class="w">T</p><p class="w">U</p><p class="w">V</p>
			<p class="w">W</p><p class="w">X</p><p class="w">Y</p><p class="w">Z</p><p class="w">#</p>
		</view>
		<view class="item1" v-for="(menu,index) in Menu" :key="index">
			<view class="card">
				<img :src="menu.avatar"></img>
				<text>{{menu.name}}</text>
			</view>
			<view class="line"></view>
		</view>
		<view class="line1">
			<view class="zi">C</view>
		</view>
		<view class="item" v-for="(menu1,index) in Menu1" :key="index">
			<view class="card">
				<img :src="menu1.avatar"></img>
				<text>{{menu1.name}}</text>
			</view>
			<view class="line"></view>
		</view>
		<view class="line1">
			<view class="zi">L</view>
		</view>
		<view class="item" v-for="(menu2,index) in Menu2" :key="index">
			<view class="card">
				<img :src="menu2.avatar"></img>
				<text>{{menu2.name}}</text>
			</view>
			<view class="line"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Menu: [{
						avatar:'../../static/xindepengy.png',
						name: '新的朋友'
					},
					{
						avatar:'../../static/qunliao.png',
						name: '群聊'
					},
					{
						avatar:'../../static/biaoqian.png',
						name: '标签'
					},
					{
						avatar:'../../static/gongzhonghao.png',
						name: '公众号'
					},
				],
				Menu1: [{
						avatar:'../../static/head7.jpg',
						name: '范同学'
					},
					{
						avatar: '../../static/head8.jpg',
						name: '蔡同学'
					}
				],
				Menu2: [{
						avatar: '../../static/head9.jpg',
						name: '施同学'
					},
					{
						avatar: '../../static/head10.jpg',
						name: '李同学'
					},
				]
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.header{
		display: flex;
		justify-content: space-between;
	}
	
	.leftright{
		display: flex;
		justify-content: space-between;
	}
	
	.left{
		display: flex;
		margin-left: 20upx;
		margin-top: 20upx;
	}
	
	.right{
		display: flex;
		margin-right: 20upx;
		margin-top: 20upx;
	}
	
	.right image{
		width: 50upx;
		height: 50upx;
	}
	
	.content {
		height: 500upx;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}

	.item1 {
		height: 100upx;
	}
	
	.logo{
		margin-right: 20upx;
	}

	.card {
		display: flex;
		align-items: center;
		margin-left: 20upx;
	}

	.card img {
		align-items: center;
		margin-top: 25upx;
		margin-right: 10upx;
		width: 80upx;
		height: 80upx;
		margin-bottom: 10upx;
	}
	
	.zimu{
	position: fixed;
	display: flex;
	height: 100%;
	margin-left: 700upx;
	flex-direction: column;
	align-items: center;
	}
	
	.line {
		height: 1upx;
		width: 100%;
		background:	#eee;
	}
	
	.line1 {
		height: 50upx;
		width: 100%;
		background:	#eee;
	}
	
	
</style>
z