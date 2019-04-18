<template>
	<view class="container">
		<view class="list1">
			<view class="list1-item">
				<text class="name">原密码为:
				{{password}}
				</text>
			</view>
			<view class="list1-item">
				<text class="name">新密码为:</text>
				<input
					class="uni-input"
					type="text"
					placeholder="输入新名称"
					v-model="userDTO.nickname"
					required="required"
				/>
			</view>
			<button type="primary" @tap="queren(userDTO)">确定</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickname: uni.getStorageSync('login_key').nickname,
				avatar: uni.getStorageSync('login_key').avatar,
				userId: uni.getStorageSync('login_key').userId,
				password: uni.getStorageSync('login_key').password
			}
			
			userDTO: {
				password: ''
			}
		},
		onLoad() {
			uni.setNavigationBarTitle({
				title: '修改昵称'
			});
		},
		methods: {
			queren: function(userDTO) {
				var _this = this;
				// console.log(userDTO.mobile + ',' + userDTO.password);
				uni.request({
					url: this.apiServer +'/user/queren',
					method: 'POST',
					data: {
						nickname: userDTO.nickname
					},
					header: {
						'content-type': 'application/json'
					},
					success: res => {
						// console.log(res.data.data);
						if (res.data.code == 0) {
							//将用户数据记录在本地存储
							uni.setStorageSync('login_key', {
								userId: res.data.data.id,
								nickname: res.data.data.nickname,
								avatar: res.data.data.avatar,
								token: res.data.data.token,
								login: true
							});
							uni.showToast({
								title: '昵称修改'
							});
							uni.navigateBack();
						}
					}
				});
			}
		}
}
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
		background-color: #fff;
		border-bottom: 1px solid #eee;
		
	}
	.name{
		margin-top: 30upx;
		margin-bottom: 30upx;
		margin-left: 20upx;
		margin-right: 50upx;
		text-align: center;
	}
</style>
