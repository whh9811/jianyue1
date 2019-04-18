<template>
	<view class="uni-flex uni-column container">
		<view class="item-nick">您之前的昵称是：{{nickname}}</view>

		<view class="item-newNN">
			<input
				class="uni-input"
				type="text"
				placeholder="请输入新的昵称"
				v-model="user.nickname1"
				required="required"
			/>

			<button class="green-btn" @tap="updateNickName(user)">确认</button>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			nickname: uni.getStorageSync('login_key').nickname,
			user: {
				nickname1: '',
				id: uni.getStorageSync('login_key').userId,
			},
		};
	},
	onShow() {
	var _this = this;
	uni.request({
		url: this.apiServer + '/user/' + uni.getStorageSync('login_key').userId,
		method: 'GET',
		header: { 'content-type': 'application/json' },
		success: res => {
			if (res.data.code === 0) {
				console.log(res.data.data.nickname+'————————————');
				_this.avatar = res.data.data.avatar;
				_this.nickname = res.data.data.nickname;
			}
		}
	});
	},
	methods: {
		updateNickName: function(user) {
			// const loginKey = uni.getStorageSync('login_key');
			var _this = this;
			uni.request({
				url: this.apiServer+'/user/updateNickName',
				// url: 'http://172.20.10.4:8080/api/user/sign_in',
				method: 'PUT',
				data: {
					nickname: _this.user.nickname1,
					// nickname:uni.getStorageSync('login_key').nickname,
					// id: uni.getStorageSync('login_key').id
					id:_this.user.id
				},
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				success: res => {
// 					console.log(user.nickname1);
// 					console.log(user.id);
					console.log(res.data.code);
					// if (res.data.code == 0) {
						uni.showToast({
							title: '修改昵称成功',
							icon: 'succes',
							duration: 1000,
							mask: true
						});
						uni.navigateBack();
// 						uni.redirectTo({
// 							url: "../user_info/user_info"
// 						});
				}
			});
		}
	}
};
</script>

<style>
.item-nick {
	margin-top: 10px;
}
.item-newNN {
	margin-top: 20px;
}
input {
	height: 50px;
	border-bottom: 1px solid #eee;
	margin-bottom: 5px;
}
.green-btn {
	background-color: #00b26a;
	margin-top: 20px;
}
</style>