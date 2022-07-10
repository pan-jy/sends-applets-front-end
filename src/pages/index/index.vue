<template>
	<view class="content">
		<view class="launch flex-col" v-if="check">
			<view class="head flex-col" :class="{checking: check}">
				<image src="../../static/logo.svg" mode=""></image>
				<text>桑 梓 微 校 园</text>
			</view>
			<view class="loading">
				<u-loading-icon :text="msg" size="20" text-size="18"></u-loading-icon>
			</view>
		</view>
		<view class="login" v-else>
			<view class="head flex-col">
				<image src="../../static/logo.svg" mode=""></image>
				<text>桑 梓 微 校 园</text>
			</view>
			<view class="input flex-col">
				<u--input v-model="account" placeholder="账号" suffixIcon="account" suffixIconStyle="color: #909399"
					type="number"></u--input>
				<u--input v-model="password" placeholder="密码" suffixIcon="lock" suffixIconStyle="color: #909399"
					:password="true" :adjustPosition="false"></u--input>
				<button class="login-btn" @click="login()">登录</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				check: true,
				msg: "Checking for Update…",
				account: '',
				password: ''
			}
		},
		onLoad: function() {
			let that = this
			const updateManager = uni.getUpdateManager();

			updateManager.onCheckForUpdate(function(res) {
				if (res.hasUpdate) {
					that.msg = "Updating..."
					updateManager.onUpdateReady(function(res) {
						uni.showModal({
							title: '更新提示',
							content: '新版本已经准备好，是否重启应用？',
							success(res) {
								if (res.confirm) {
									// 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
									updateManager.applyUpdate();
								}
							}
						});
					});

					updateManager.onUpdateFailed(function(res) {
						// 新的版本下载失败
						uni.showModal({
							title: '更新提示',
							content: '新版本下载失败，请重启应用',
							showCancel: false,
							success(res) {
								if (res.confirm) {
									// 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
									updateManager.applyUpdate();
								}
							}
						});
					});
				} else {
					that.check = false
				}
			});
		},
		methods: {
			login(){
				uni.switchTab({
					url: "../home/home"
				})
			}
		}
	}
</script>

<style lang="scss">
	.content,
	.launch {
		height: 100%;
	}

	.head {
		height: 40vh;
		align-items: center;
		font-size: 28px;
		font-weight: 100;
	}

	.checking {
		margin-top: 10vh;
	}

	.loading {
		margin-top: 30vh;
	}

	.u-loading-icon__text {
		margin-left: 10px !important;
	}

	.input {
		margin-top: 3vh;
		align-items: center;

		input {
			height: 5vh !important;
			line-height: 5vh !important;
			font-size: 16px !important;
		}

		.u-icon__icon {
			line-height: 5vh !important;
			font-size: 25px !important;
		}

		.u-input {
			width: 60vw !important;
			margin-bottom: 3vh;
			border-radius: 12px !important;
		}
	}

	.login-btn {
		width: 50vw;
		height: 7vh;
		background-color: #f1f5f9;
		border-radius: 15px;
		font-weight: 200;
	}
</style>
