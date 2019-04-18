<template>
	<view class="container">
		<view class="sign-box">
			<input class="uni-input left" type="number" placeholder="输入手机号" v-model="mobile" required="required" />
				<button class="green-btn1 small-btn right" v-show="go1" @tap="getVerifyCode">
					点此获取验证码
				</button>
				<button class="green-btn2 small-btn right" v-show="go2" >
					<uni-countdown :show-day="false" :second="60" @timeup="timeup"></uni-countdown>
					获取验证码
				</button>
		</view>
		<input class="uni-input" type="number" placeholder="输入验证码" v-model="verifyCode" required="required" />
		<button @tap="checkCode" class="green-btn">下一步</button>
	</view>
</template>
<script>
	import uniCountdown from "@/components/uni-countdown/uni-countdown.vue";
	export default {
	components: {uniCountdown},
	data() {
		return {
			go1:true,
			go2:false,
			mobile: '',
			verifyCode: ''
		};
	},
	onLoad() {},
	methods: {
		getVerifyCode: function() {
			var _this = this;
			this.go1=false,
			this.go2=true,
			uni.request({
				url: this.apiServer + '/user/verify',
				method: 'POST',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					mobile: _this.mobile
				},
				success: res => {
					if (res.data.code === 0) {
						uni.showToast({
							title: '验证码已发送'
						});
						_this.disabled = true;
						console.log(_this.disabled);
					} else {
						uni.showModal({
							title: '提示',
							content: res.data.msg
						});
					}
				}
			});
		},
		checkCode: function() {
			var _this = this;
			console.log(_this.verifyCode);
			uni.request({
				url: this.apiServer + '/user/check',
				method: 'POST',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					mobile: _this.mobile,
					verifyCode: _this.verifyCode
				},
				success: res => {
					console.log(res.data);
					if (res.data.code === 0) {
						uni.navigateTo({
							url: '../password/password?mobile=' + _this.mobile
						});
					} else {
						uni.showModal({
							title: '提示',
							content: res.data.msg
						});
					}
				}
			});
		}
	}
};
</script>

<style>
	.uni-input{
		height: 100upx;
	}
.sign-box {
	display: flex;
	align-items: center;
}
.left {
	flex: 1 1 70%;
}
.green-btn1{
	color: #FFFFFF;
	background: linear-gradient(40deg, #AFEEEE, #008080);
}
.green-btn2{
	color: #FFFFFF;
	background: #808080;
}
.small-btn {
	display: flex;
	text-align: center;
	width: 130px;
	height: 35px;
	font-size: 14px;
}
</style>