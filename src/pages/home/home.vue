<template>
	<view class="content">
		<view class="calendar margin-b" style="padding-top: 15px;">
			<uni-card margin="0 15px 15px 15px">
				<template v-slot:title>
					<view class="head flex-row">
						<view>
							<u-datetime-picker :show="picker" v-model="date" mode="year-month" @confirm="picker = false"
								@cancel="picker = false" closeOnClickOverlay @close="picker = false"
								:maxDate="staticDate + 60*60*24*365*2*1000" :minDate="staticDate - 60*60*24*365*2*1000">
							</u-datetime-picker>
							<view @click="picker = true" class="flex-row">
								<view class="head-view year">
									<text>{{year}}</text>
								</view>
								<view class="head-view month">
									<text>{{month}}月</text>
								</view>
								<u-icon v-if="picker" name="arrow-up-fill"></u-icon>
								<u-icon v-else name="arrow-down-fill"></u-icon>
							</view>
						</view>
						<view class="head-view week">
							<text>第{{week}}周</text>
						</view>
					</view>
				</template>
				<view>
					<view class="calendar-item flex-row week-head">
						<view>Mo</view>
						<view>Tu</view>
						<view>We</view>
						<view>Th</view>
						<view>Fr</view>
						<view>Sa</view>
						<view>Su</view>
					</view>
					<view class="calendar-item flex-row">
						<swiper class="swiper" circular @change="swiper" :current="current" :duration="0">
							<swiper-item class="flex-row" v-for="(dayArr,index) in weekArr" :key="index">
								<view v-for="(day,index2) in dayArr" :key="index2" @click="pickDay(index2)"
									:class="{'today':day == new Date(staticDate).getDate() && year == new Date(staticDate).getFullYear() && new Date(date).getMonth() == new Date(staticDate).getMonth(),
									 'selected':index2 == currentIndex,'today-act':day == new Date(staticDate).getDate() && year == new Date(staticDate).getFullYear() && new Date(date).getMonth() == new Date(staticDate).getMonth() && index2 == currentIndex}"
									class="day flex-col">
									{{day}}
								</view>
							</swiper-item>
						</swiper>
					</view>
				</view>
			</uni-card>
		</view>
		<view class="default" v-if="!selected">
			<view class="navigation-bar margin-b">
				<uni-card>
					<view style="margin: 2vw 0 1vh 0;">
						<u-grid :border="false" :col="4">
							<u-grid-item v-for="(listItem,baseListIndex) in baseList" :key="baseListIndex">
								<image :src="listItem.src" mode="scaleToFill"
									style="height: 5vh;width: 20vw;margin-bottom: 1vh;"></image>
								<text class="grid-text" style="font-size: 15px;">{{listItem.title}}</text>
							</u-grid-item>
						</u-grid>
					</view>
				</uni-card>
			</view>
			<view class="market">
				<view class="head flex-row">
					<view>
						<text style="font-size: 20px;">校园集市</text>
					</view>
					<view class="flex-row">
						<text style="font-size: 15px;color: #909399;">查看更多</text>
						<u-icon name="arrow-right" color="#909399" size="15"></u-icon>
					</view>
				</view>
				<view class="market-item">
					<uni-card is-full :is-shadow="false" v-for="item in marketItemList" :key="item.name"
						margin="0 0 15px 0">
						<u-skeleton rows="2" :loading="loading" avatar avatarShape="square" avatarSize="12vw"
							:rowsHeight="rowsHeight">
							<view class="flex-row">
								<view style="width: 15vw;">
									<u-avatar :src="item.avatar" shape="square" size="12vw"></u-avatar>
								</view>
								<view class="flex-fill">
									<view class="flex-row"
										style="justify-content: space-between;font-size: 20px;margin-bottom: 2vh;">
										<view>{{item.name}}</view>
										<view>
											<u-icon name="more-dot-fill" :size="18"></u-icon>
										</view>
									</view>
									<view style="margin-bottom: 1vh;font-size: 16px;">
										<text>{{item.content}}</text>
									</view>
									<view class="flex-row"
										style="justify-content: space-between;align-items: center;font-size: 12px;">
										<view style="background-color: #f1f2f4;border-radius: 5px;">#{{item.type}}
										</view>
										<view>{{formatTime(parseInt(item.time))}}</view>
										<view class="flex-row">
											<view class="flex-row" style="margin-right: 1vw;">
												<u-icon name="heart" :size="20"></u-icon>
												<text>{{item.like}}</text>
											</view>
											<view class="flex-row" style="margin-right: 1vw;">
												<u-icon name="chat" :size="20"></u-icon>
												<text>{{item.comment}}</text>
											</view>
											<view class="flex-row">
												<u-icon name="star" :size="20"></u-icon>
												<text>{{item.star}}</text>
											</view>
										</view>
									</view>
								</view>
							</view>
						</u-skeleton>
					</uni-card>
				</view>
			</view>
		</view>
		<view class="classSchedule" v-else>
			<view class="flex-row">
				<view class="fab flex-row" @click="fabClick = !fabClick"
					:class="{'fab-rotate':fabClick,'fab-contrarotate':!fabClick}">
					<u-icon name="plus" size="7vw" color="#fff"></u-icon>
				</view>
				<view class="fab-content flex-row" v-if="fabClick">
					<view class="fab-content-item flex-col" @click="backHome">
						<u-icon name="home" size="6vw"></u-icon>
						<text class="grid-text" style="font-size: 10px;">返回首页</text>
					</view>
					<view class="fab-content-item flex-col" @click="addTransaction">
						<u-icon name="plus" size="6vw"></u-icon>
						<text class="grid-text" style="font-size: 10px;">添加事件</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		uniCard,
		uniFab
	} from '@dcloudio/uni-ui'

	export default {
		data() {
			return {
				date: Number(new Date()),
				staticDate: Number(new Date()),
				picker: false, //日期选择框
				week: 'XX',
				current: 1, //当前weekArr下标
				currentIndex: Number(new Date().getDay() - 1),
				selected: false,
				baseList: [{
					src: '../../static/home/music.svg',
					title: '点歌台'
				}, {
					src: '../../static/home/grade.svg',
					title: '成绩查询'
				}, {
					src: '../../static/home/bus.svg',
					title: '校园小蓝'
				}, {
					src: '../../static/home/navigation.svg',
					title: '校内导航'
				}],
				rowsHeight: ['15vh'],
				loading: true,
				marketItemList: [{
					name: '李华',
					avatar: '../../static/tab/profile.png',
					content: '遗忘是一般刚强的，有创造力的人的法宝，他们会像自然一样的遗忘，自然界就不知道有什么过失，弱者不是把痛苦作为惩前毖后的教训，反而在痛苦中讨生活，浸在里头，天天回顾以往的苦难，折磨自己。——巴尔扎克',
					type: '打听求助',
					time: '1656470528000',
					like: '13',
					comment: '4',
					star: '4'
				}, {
					name: '小明',
					avatar: '../../static/tab/profile.png',
					content: '感到自己是人们所需要的和亲近的人——这是生活最大的享受，最高的喜悦。这是真理，不要忘记这个真理，它会给你们无限的幸福。——高尔基',
					type: '打听求助',
					time: '1656480528000',
					like: '7',
					comment: '2',
					star: '1'
				}],
				fabClick: false
			}
		},
		onLoad() {
			// 模拟请求
			uni.$u.sleep(2000).then(() => {
				this.loading = false
			})
		},
		components: {
			uniCard,
			uniFab
		},
		computed: {
			year() {
				let date = new Date(parseInt(this.date));
				let year = date.getFullYear();
				return year;
			},
			month() {
				let date = new Date(parseInt(this.date));
				let month = date.getMonth() + 1;
				switch (month) {
					case 1:
						month = '一'
						break;
					case 2:
						month = '二'
						break;
					case 3:
						month = '三'
						break;
					case 4:
						month = '四'
						break;
					case 5:
						month = '五'
						break;
					case 6:
						month = '六'
						break;
					case 7:
						month = '七'
						break;
					case 8:
						month = '八'
						break;
					case 9:
						month = '九'
						break;
					case 10:
						month = '十'
						break;
					case 11:
						month = '十一'
						break;
					case 12:
						month = '十二'
						break;
				}
				return month;
			},
			weekArr() {
				let date = new Date(parseInt(this.date));
				let day = date.getDate();
				let week = date.getDay();
				let weekArr = [
					[],
					[],
					[]
				];
				for (let i = 0; i < 7; i++) {
					weekArr[0][i] = new Date(parseInt(this.date - (6 + week - i) * 60 * 60 * 24 * 1000)).getDate()
				}
				weekArr[1][week - 1] = day
				for (let i = week - 2, j = 1; i >= 0; i--, j++) {
					weekArr[1][i] = new Date(parseInt(this.date - j * 60 * 60 * 24 * 1000)).getDate()
				}
				for (let i = week, j = 1; i < 7; i++, j++) {
					weekArr[1][i] = new Date(parseInt(this.date + j * 60 * 60 * 24 * 1000)).getDate()
				}
				for (let i = 0; i < 7; i++) {
					weekArr[2][i] = new Date(parseInt(this.date + (8 - week + i) * 60 * 60 * 24 * 1000)).getDate()
				}
				// console.log(weekArr)
				return weekArr;
			}
		},
		methods: {
			swiper(e) {
				let date = new Date(parseInt(this.date));
				let week = date.getDay();
				if (e.detail.current == 2) {
					this.date += (8 - week) * 60 * 60 * 24 * 1000
				} else if (e.detail.current == 0) {
					this.date -= (6 + week) * 60 * 60 * 24 * 1000
				}
				this.current = e.detail.current
				this.$nextTick(function() {
					this.current = 1
				});
			},
			pickDay(index) {
				this.currentIndex = index
				this.selected = true
			},
			formatTime(time) {
				let data = new Date(parseInt(time));
				let year = data.getFullYear();
				let month = data.getMonth() + 1;
				let day = data.getDate();
				let h = data.getHours();
				let m = data.getMinutes();
				// let s = data.getSeconds();
				month = month >= 10 ? month : "0" + month;
				day = day >= 10 ? day : "0" + day;
				h = h >= 10 ? h : "0" + h;
				m = m >= 10 ? m : "0" + m;
				return year + "/" + month + "/" + day + " " + h + ":" + m;
			},
			backHome() {
				this.selected = false
				this.currentIndex = Number(new Date().getDay() - 1)
				this.fabClick = false
			},
			addTransaction() {
				console.log("添加事务")
			}
		}
	}
</script>

<style lang="scss">
	.content {
		min-height: 100%;
		background-image: linear-gradient(to bottom, #fff, #ecf5ff);
	}

	.margin-b {
		margin-bottom: 4vh;
	}

	.uni-card {
		border-radius: 20px !important;
	}

	.uni-card--full {
		margin-bottom: 1vh !important;
	}

	.head {
		justify-content: space-between;
		align-items: center;
		margin: 2vw;
	}

	.head-view {
		height: 5vh;
		line-height: 5vh;
		font-size: 20px;
		margin-right: 2vw;
	}

	.week {
		margin-right: 0;
	}

	.calendar-item {
		justify-content: space-between;
		font-size: 22px;

		view {
			width: 10vw;
			text-align: center;
			border-radius: 100px;
			padding: 1vh;
		}

		.day {
			justify-content: center;
		}
	}

	.swiper {
		width: 100%;
		height: 7vh;
	}

	.week-head {
		color: #c4c6c9;
		margin-bottom: 1vh;
	}

	.today {
		color: #398ade;
	}

	.today-act {
		background-color: #398ade;
		color: #fff;
	}

	.selected {
		border: #398ade 1px solid;
	}

	.fab {
		width: 13vw;
		height: 13vw;
		background-color: #398ade;
		border-radius: 100px;
		position: fixed;
		right: 5%;
		bottom: 5%;
		z-index: 99;
		box-shadow: 0 1px 5px 2px rgb(0 0 0 / 30%);
		justify-content: center;
		align-items: center;
	}

	.fab-rotate {
		transform: rotate(45deg);
		transition: .2s;
	}

	.fab-contrarotate {
		transform: rotate(0deg);
		transition: .2s;
	}

	.fab-content {
		position: absolute;
		right: 12.5%;
		bottom: 5%;
		height: 13vw;
		padding: 0 6.5% 0 5.5%;
		background-color: #fff;
		border-top-left-radius: 55px;
		border-bottom-left-radius: 55px;
		box-shadow: 0 1px 5px 2px rgb(0 0 0 / 30%);
	}

	.fab-content-item {
		width: 13vw;
		justify-content: space-around;
		align-items: center;
		margin: 1vw;
	}
</style>
