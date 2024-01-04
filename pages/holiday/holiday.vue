<template>
	<view class="bg-white">
		<!-- 在<view class="bg-white">元素内部，使用v-for指令循环遍历showHolidays数组，并为每个项目渲染一个<view class="cu-timeline">元素。 -->
		<view class="cu-timeline" v-for='item in showHolidays' v-bind:key='item.key' >
			<view class="cu-time">{{new Date(item.jiaDayBegin).format("MM-dd")}}</view>
			<view class="cu-item " :class="{'text-blue cuIcon-roundcheckfill':item.cur,'cuIcon-emoji':(!item.cur&&item.ju<0),'cuIcon-loading':(!item.cur&&item.ju>0)}">
				<view class="content shadow-blur" :class="{'bg-green light':item.cur,'bg-blue light':(!item.cur&&item.ju>0)}">
					<view>
						<text v-if="item.ju > 0" class="cu-tag badge round light  line-red"><text class="text-df">{{item.ju}}</text>天</text>
						<view>
							<text class="text-red text-shadow text-bold padding-right-xs">{{item.name}}</text>
							<text>放假<text class="text-orange text-lg text-bold padding-right-xs padding-left-xs">{{item.jia}}</text>天</text>
						</view>
						<view>
							<text>假期：</text><text>{{new Date(item.jiaDayBegin).format("MM月dd日")}}-{{new Date(item.jiaDayEnd).format("MM月dd日")}}</text>
						</view>
						<view>
							<text v-if="item.bu > 0">补班：</text><text>{{item.buDays.join('、')}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	const {
		Solar,
		HolidayUtil
	} = require('@/common/lunar.js');

	export default {
		data() {
			return {
				showHolidays: []
			}
		},
		onLoad() {
			this.render(); 			// 在组件加载时执行的逻辑
		},
		onReady() {
			this.scrollPosition();  // 在组件准备完毕时执行的逻辑
		},
		onShow() {
			this.scrollPosition(); 	// 在组件显示时执行的逻辑
		},
		methods: {
			render() {				// 渲染假期数据的方法
				this.showHolidays = [];
				let today = Solar.fromDate(new Date());
				let holidays = HolidayUtil.getHolidays(new Date().getFullYear());
				// 遍历
				let curTarget = '';
				let jiaDayBegin = '';
				let jiaDayEnd = '';
				let buDays = [];
				let curName = '';
				let curBuNum = 0;
				let curJiaNum = 0;
				let isCurHo = false;
				for (var i = 0; i < holidays.length; i++) {
					// console.log(holidays[i]);
					// 如果与上一次相同
					if (holidays[i].getTarget() == curTarget) {
						curTarget = holidays[i].getTarget();
						if (holidays[i].isWork()) {
							curBuNum++;
							buDays.push(new Date(holidays[i].getDay()).format("MM月dd日"));
						} else {
							curJiaNum++;
							if (jiaDayBegin == '') {
								jiaDayBegin = holidays[i].getDay();
							}
							jiaDayEnd = holidays[i].getDay();
						}
					}
					// 如果这是一个新假期
					if (holidays[i].getTarget() != curTarget) {
						if (curTarget != '') {
							let d = ((new Date(jiaDayBegin) - new Date(today)) / (1000 * 60 * 60 * 24));
							let isC = false;
							if (!isCurHo && d >= 0) {
								isCurHo = true;
								isC = true;
							}
							this.showHolidays.push({
								"key":i,
								"name": curName,
								"target": curTarget,
								"bu": curBuNum,
								"jia": curJiaNum,
								"jiaDayBegin": jiaDayBegin,
								"jiaDayEnd": jiaDayEnd,
								"buDays": buDays,
								"ju": d,
								"cur": isC
							})
						}
						curTarget = holidays[i].getTarget();
						curName = holidays[i].getName();
						jiaDayBegin = '';
						jiaDayEnd = '';
						buDays = [];
						if (holidays[i].isWork()) {
							curBuNum = 1;
							curJiaNum = 0;
							buDays.push(new Date(holidays[i].getDay()).format("MM月dd日"));
						} else {
							curBuNum = 0;
							curJiaNum = 1;
							if (jiaDayBegin == '') {
								jiaDayBegin = holidays[i].getDay();
							}
							jiaDayEnd = holidays[i].getDay();
						}
					}
					// 如果这是最后一个循环
					if (i == holidays.length - 1) {
						if (curTarget != '') {
							let d = (new Date(jiaDayBegin) - new Date(today)) / (1000 * 60 * 60 * 24);
							let isC = false;
							if (!isCurHo && d >= 0) {
								isCurHo = true;
								isC = true;
							}
							this.showHolidays.push({
								"key":i,
								"name": curName,
								"target": curTarget,
								"bu": curBuNum,
								"jia": curJiaNum,
								"jiaDayBegin": jiaDayBegin,
								"jiaDayEnd": jiaDayEnd,
								"buDays": buDays,
								"ju": d,
								"cur": isC
							})
						}
					}
				}
			},
			formatDay(dayStr){
				return 
			},
			// onShareAppMessage()和onShareTimeline()方法定义了在分享组件时的行为。它们返回一个包含所需标题、路径和mpId（小程序ID）的对象来进行分享。
			onShareAppMessage(res) {
			    if (res.from === 'button') {// 来自页面内分享按钮
			      console.log(res.target)
			    }
			    return {
			      title: '离放假还有几天',
			      path: '/pages/holiday/holiday',
				  mpId: 'wxee05508955140bfd'
			    }
			  },
			onShareTimeline(res){
			  if (res.from === 'button') {// 来自页面内分享按钮
				console.log(res.target)
			  }
			  return {
				title: '离放假还有几天',
				path: '/pages/holiday/holiday',
				mpId: 'wxee05508955140bfd'
			  }
		  },
		//   scrollPosition()方法将页面滚动到顶部，然后使用uni.createSelectorQuery()选择第一个具有"class"cuIcon-roundcheckfill"的元素。如果找到该元素，则滚动到该元素的顶部。
		  scrollPosition(){
			  uni.pageScrollTo({
			  	scrollTop:0,
			  	duration:0
			  })
			 uni.createSelectorQuery().select(".cuIcon-roundcheckfill").boundingClientRect(function(res){
				if(res){
					uni.pageScrollTo({
						scrollTop:res.top,
						duration:300
					})
				}
			 }).exec();
		  },
		}
	}
</script>

<style>

</style>
