<template>
	<view class="content">
		<view style="padding-top: 15px;">
			<uni-card margin="0 15px 15px 15px">
				<view>
					<view class="flex-row profile">
						<view>个人信息</view>
						<view v-if="!edit"><button class="btn" @click="editClick">更改</button></view>
						<view class="flex-row" v-else>
							<button class="btn" @click="submit">确定</button>
							<button class="btn" @click="editCancle">取消</button>
						</view>
					</view>
					<view>
						<u--form labelPosition="left" :model="userInfo" :rules="rules" ref="form" labelWidth="100"
							:labelStyle="labelStyle">
							<u-form-item label="头像：" prop="avatar" borderBottom ref="item">
								<u-avatar :src="userInfo.avatar" shape="square" size="10vw"></u-avatar>
								<!-- <u--input v-model="userInfo.avatar" :disabled="disabled" disabledColor="#ffffff"
									fontSize="17px" :color="inputColor" border="none"></u--input> -->
							</u-form-item>
							<u-form-item label="姓名：" prop="name" borderBottom ref="item">
								<u--input v-model="userInfo.name" :disabled="disabled" disabledColor="#ffffff"
									fontSize="17px" :color="inputColor" border="none"></u--input>
							</u-form-item>
							<u-form-item label="校区：" prop="campus" borderBottom
								@click="showCampus = edit ? true : false;" ref="item">
								<u--input v-model="userInfo.campus" disabled disabledColor="#ffffff" fontSize="17px"
									:color="inputColor" placeholder="请选择校区" border="none"></u--input>
								<u-icon slot="right" name="arrow-right"></u-icon>
							</u-form-item>
							<u-form-item label="电子邮箱：" prop="mailbox" ref="item">
								<u--input v-model="userInfo.mailbox" :disabled="disabled" disabledColor="#ffffff"
									fontSize="17px" :color="inputColor" border="none"></u--input>
							</u-form-item>
						</u--form>
						<u-action-sheet :show="showCampus" :actions="campusList" title="请选择校区"
							@close="showCampus = false" @select="campusSelect">
						</u-action-sheet>
					</view>
				</view>
			</uni-card>
			<uni-card>
				<view class="settingItem">
					<text>
						关联公众号通知
					</text>
					<view>
						<u-switch v-model="switchValue" asyncChange @change="switchChange" activeColor="#53c21d"></u-switch>
					</view>
				</view>
			</uni-card>
			<uni-card>
				<view class="settingItem">
					<text>
						使用须知
					</text>
				</view>
			</uni-card>
			<uni-card>
				<view class="settingItem">
					<text>
						意见与反馈
					</text>
				</view>
			</uni-card>
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
				userInfo: {
					avatar: '/static/avatar.png',
					name: '李华',
					campus: '厦门校区',
					mailbox: 'ZZZ@zz.com'
				},
				tempInfo: {},
				edit: false,
				inputColor: '#888888',
				disabled: true,
				showCampus: false,
				campusList: [{
						name: '厦门校区',
					},
					{
						name: '泉州校区',
					},
					{
						name: '龙舟池校区',
					},
				],
				labelStyle: {
					'font-size': '17px',
					'color': '#303133'
				},
				rules: {
					'name': {
						type: 'string',
						required: true,
						message: '请输入昵称',
						trigger: ['blur', 'change']
					},
					'campus': {
						type: 'string',
						required: true,
						message: '请选择校区',
						trigger: ['blur', 'change']
					},
					'mailbox': {
						type: 'email',
						required: true,
						message: '请输入邮箱',
						trigger: ['blur', 'change']
					},
				},
				switchValue: false
			}
		},
		components: {
			uniCard
		},
		methods: {
			editClick() {
				this.tempInfo = Object.assign({}, this.userInfo);
				this.edit = true
				this.disabled = false
				this.inputColor = '#303133'
			},
			editCancle() {
				this.userInfo = Object.assign({}, this.tempInfo);
				this.$refs.form.clearValidate()
				this.edit = false
				this.disabled = true
				this.inputColor = '#888888'
			},
			campusSelect(e) {
				this.userInfo.campus = e.name
				this.$refs.form.validateField('campus')
			},
			submit() {
				this.$refs.form.validate().then(res => {
					uni.$u.toast('校验通过')
					this.edit = false;
					this.disabled = true;
					this.inputColor = '#888888'
				}).catch(errors => {
					uni.$u.toast('校验失败')
				})
			},
			switchChange(e) {
				uni.showModal({
					content: e ? '确定关联通知？' : '不再关联通知？',
					success: (res) => {
						// console.log(res)
						if (res.confirm) {
							this.switchValue = e
						}
					}
				})
			}
		}
	}
</script>

<style>
	.profile {
		height: 6vh;
		line-height: 6vh;
		justify-content: space-between;
		align-items: center;
		color: #000;
		font-size: 20px;
	}

	.btn {
		padding: 0;
		height: 4vh;
		line-height: 4vh;
		width: 15vw;
		border-radius: 20px;
		font-size: 14px;
		background-color: #fff;
		border: 1px #cbd5e1 solid;
		margin-left: 2vw;
	}

	.uni-card {
		border-radius: 15px !important;
		box-shadow: 0px 0px 5px 1px rgba(0, 0, 0, 0.1) !important;
	}

	.settingItem {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		font-size: 18px;
		height: 6vh;
		line-height: 6vh;
	}
</style>
