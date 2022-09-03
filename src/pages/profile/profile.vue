<template>
	<view class="content">
		<view class="profile-head">
			<uni-card is-full :is-shadow="false">
				<view class="flex-row" style="padding: 2vh 0 4vh 0;">
					<view style="width: 25vw;">
						<u-avatar :src="profile.avatar" shape="square" size="20vw"></u-avatar>
					</view>
					<view class="flex-fill flex-col" style="justify-content: space-between;">
						<view class="flex-row" style="justify-content: space-between;font-size: 25px;margin-top: 1vh;">
							<view>{{profile.name}}</view>
							<view @click="toSetting">
								<u-icon name="setting" :size="35"></u-icon>
							</view>
						</view>
						<view style="font-size: 18px;color: #909399;">
							<text>学号：{{profile.stuNum}}</text>
						</view>
					</view>
				</view>
			</uni-card>
		</view>
		<view class="action-cell-group">
			<view class="action-cell">
				<u-cell-group :border="false"
					customStyle="background-color: #fff;box-shadow: 0px 0px 3px 1px rgba(0, 0, 0, 0.08); border-radius: 5px">
					<u-cell title="我的集市" :isLink="true" size="large" titleStyle="font-size:20px"
						rightIconStyle="width:20px;height:20px"></u-cell>
				</u-cell-group>
			</view>
			<view class="action-cell">
				<u-cell-group :border="false"
					customStyle="background-color: #fff;box-shadow: 0px 0px 3px 1px rgba(0, 0, 0, 0.08); border-radius: 5px">
					<u-cell title="XXXXXXXX" :isLink="true" size="large" titleStyle="font-size:20px"
						rightIconStyle="width:20px;height:20px"></u-cell>
				</u-cell-group>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		uniCard
	} from '@dcloudio/uni-ui'

	export default {
		data() {
			return {
				profile: {
					name: '李华',
					avatar: '/static/avatar.png',
					stuNum: '2020202020',
				},
			}
		},
		components: {
			uniCard
		},
		methods: {
			toSetting(){
				uni.navigateTo({
					url: './setting/setting'
				})
			}
		}
	}
</script>

<style>
	.uni-card--full {
		border-bottom-left-radius: 20px !important;
		border-bottom-right-radius: 20px !important;
	}

	.profile-head {
		margin-bottom: 4vh;
	}

	.action-cell-group {
		padding: 0 2vw 0 2vw;
	}

	.action-cell {
		margin-bottom: 3vh;
	}

	.u-cell__body {
		padding-top: 18px !important;
		padding-bottom: 18px !important;
	}
</style>
