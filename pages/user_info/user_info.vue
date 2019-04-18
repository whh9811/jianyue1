<template>
	<view class="container">
		<view class="list1">
			<view class="list1-item">
				<navigator url="../newname/newname" hover-class="navigator-hover" class="name">
					<text class="name1">昵称:</text>
					{{nickname}}
				</navigator>
			</view>
			<view class="list1-item">
				<text class="name1">头像</text>
				<image :src="avatar" class="avatar" @tap="showActionSheet"></image>
			</view>
			<navigator url="../newpassword/newpassword" hover-class="navigator-hover">
				<view class="list1-item"><text class="name">修改密码</text></view>
			</navigator>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickname: uni.getStorageSync('login_key').nickname,
				avatar: uni.getStorageSync('login_key').avatar,
				userId: uni.getStorageSync('login_key').userId
			}
		},
		onLoad() {

		},
		onShow() {
		var _this = this;
		uni.request({
			url: this.apiServer + '/user/' + uni.getStorageSync('login_key').userId,
			method: 'GET',
			header: { 'content-type': 'application/json' },
			success: res => {
				if (res.data.code === 0) {
					console.log(res.data.data.avatar+'————————————');
					_this.avatar = res.data.data.avatar;
					_this.nickname = res.data.data.nickname;
				}
			}
		});
	},
		methods: {
		showActionSheet: function() {
			console.log('show');
			var _this = this;
			uni.showActionSheet({
				itemList: ['拍照', '从相册选择'],
				success: function(res) {
					console.log('选中了第' + (res.tapIndex + 1) + '个按钮');
					//选择的是拍照功能
					if (res.tapIndex == 0) {
						uni.chooseImage({
							count: 1,
							sourceType: ['camera'],
							success: function(res) {
								uni.saveImageToPhotosAlbum({
									filePath: res.tempFilePaths[0],
									success: function() {
										console.log('save success');
										uni.uploadFile({
											url: 'http://192.168.43.68:8080/api/user/avatar', //仅为示例，非真实的接口地址
											filePath: res.tempFilePaths[0],
											name: 'file',
											formData: {
												userId: _this.userId
											},
											success: uploadFileRes => {
												console.log(uploadFileRes.data);
												_this.avatar = uploadFileRes.data;
											}
										});
									}
								});
							}
						});
					}
					//从相册选择
					if (res.tapIndex == 1) {
						uni.chooseImage({
							count: 1, //默认9
							sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都
							sourceType: ['album'], //从相册选择
							success: function(res) {
								console.log(JSON.stringify(res.tempFilePaths));
								uni.uploadFile({
									url: 'http://192.168.43.68:8080/api/user/avatar', //仅为示例，非真实的接口地址
									filePath: res.tempFilePaths[0],
									name: 'file',
									formData: {
										userId: _this.userId
									},
									success: uploadFileRes => {
										console.log(uploadFileRes.data);
										_this.avatar = uploadFileRes.data;
									}
								});
							}
						});
					}
				},
				fail: function(res) {
					console.log(res.errMsg);
				}
			});
		}
	}
};
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
	}
	.list1 {
		display: flex;
		flex-direction: column;
	}
	.list1-item {
		display: flex;
		align-items: center;
		width: 100%;
		margin-top: 20upx;
		margin-left: 20upx;
		margin-bottom: 20upx;
		background-color: #fff;
		border-bottom: 1px solid #eee;
	}
	.name1{
		margin-right: 50upx;
		text-align: center;
	}
	.name{
		margin-bottom: 20upx;
	}
</style>