<template>
	<view>
		<uni-card>
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
			<view class="content">
				<view class="content-view flex-row week-head">
					<view>Mo</view>
					<view>Tu</view>
					<view>We</view>
					<view>Th</view>
					<view>Fr</view>
					<view>Sa</view>
					<view>Su</view>
				</view>
				<view class="content-view flex-row">
					<swiper class="swiper" circular @change="swiper" :current="current" :duration="0">
						<swiper-item class="flex-row" v-for="(dayArr,index) in weekArr" :key="index">
							<view v-for="day in dayArr" :key="day"
								:class="{'today':day == new Date(staticDate).getDate() && year == new Date(staticDate).getFullYear() && new Date(date).getMonth() == new Date(staticDate).getMonth()}"
								class="day flex-col">
								{{day}}
							</view>
						</swiper-item>
					</swiper>
				</view>
			</view>
		</uni-card>
	</view>
</template>

<script>
	import {
		uniCard
	} from '@dcloudio/uni-ui'

	export default {
		data() {
			return {
				date: Number(new Date()),
				staticDate: Number(new Date()),
				picker: false,
				week: '18',
				current: 1
			}
		},
		components: {
			uniCard
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
				console.log(weekArr)
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
			}
		}
	}
</script>

<style lang="scss">
	.uni-card {
		border-radius: 20px !important;
	}

	.head {
		justify-content: space-between;
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

	.content-view {
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
		color: #cbd5e1;
		margin-bottom: 1vh;
	}

	.today {
		background-color: #adadad;
		color: #fff;
	}
</style>
