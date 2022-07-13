<template>
	<view class="content">
		<view class="tools-item flex-col border-bottom">
			<view class="item-title">桑梓助手</view>
			<view>
				<u-grid :border="false" :col="4">
					<u-grid-item v-for="(listItem,baseListIndex) in baseList[0]" :key="baseListIndex">
						<image class="item-icon" :src="listItem.src" mode="scaleToFill">
						</image>
						<text class="item-text">{{listItem.title}}</text>
					</u-grid-item>
				</u-grid>
			</view>
		</view>
		<view class="tools-item flex-col border-bottom">
			<view class="item-title">教务平台</view>
			<view>
				<u-grid :border="false" :col="4">
					<u-grid-item v-for="(listItem,baseListIndex) in baseList[1]" :key="baseListIndex">
						<image class="item-icon" :src="listItem.src" mode="scaleToFill">
						</image>
						<text class="item-text">{{listItem.title}}</text>
					</u-grid-item>
				</u-grid>
			</view>
		</view>
		<view class="tools-item flex-col">
			<view class="item-title">校园卡</view>
			<view>
				<u-grid :border="false" :col="4">
					<u-grid-item v-for="(listItem,baseListIndex) in baseList[2]" :key="baseListIndex">
						<image class="item-icon" :src="listItem.src" mode="scaleToFill">
						</image>
						<text class="item-text">{{listItem.title}}</text>
					</u-grid-item>
				</u-grid>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				baseList: [
					[{
						src: '../../static/tools/market.svg',
						title: '校园集市'
					}, {
						src: '../../static/tools/music.svg',
						title: '点歌台'
					}, {
						src: '../../static/tools/navigation.svg',
						title: '校内导航'
					}, {
						src: '../../static/tools/bus.svg',
						title: '校园小蓝'
					}, {
						src: '../../static/tools/actList.svg',
						title: '活动列表'
					}, {
						src: '../../static/tools/applyAct.svg',
						title: '活动申请'
					}],
					[{
						src: '../../static/tools/schedule.svg',
						title: '课表查询'
					}, {
						src: '../../static/tools/door.svg',
						title: '空教室申请'
					}, {
						src: '../../static/tools/arrange.svg',
						title: '考试安排'
					}, {
						src: '../../static/tools/run.svg',
						title: '体测成绩'
					}, {
						src: '../../static/tools/grade.svg',
						title: '成绩查询'
					}, {
						src: '../../static/tools/star.svg',
						title: '学分查询'
					}],
					[{
						src: '../../static/tools/card.svg',
						title: '校园卡'
					}, {
						src: '../../static/tools/reserve.svg',
						title: '场馆预定'
					}, {
						src: '../../static/tools/fix.svg',
						title: '宿舍报修'
					}]
				]
			}
		},
		methods: {

		}
	}
</script>

<style>
	.tools-item {
		padding: 4vw;
		padding-bottom: 0;
	}

	.border-bottom {
		border-bottom: 1px #e7e6e4 solid;
	}

	.item-title {
		font-size: 20px;
		margin-bottom: 5vw;
	}

	.item-icon {
		height: 7vh;
		width: 20vw;
		margin-bottom: 1vh;
	}

	.item-text {
		font-size: 16px;
		margin-bottom: 4vw;
	}
</style>
