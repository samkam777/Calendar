<!-- 在模板（template）部分，定义了组件的结构和布局。它包含一个名为Calendar的自定义组件，以及一些视图（view）和样式（style）的定义。 -->
<!-- <template>标签用于定义组件的模板部分。
	<view>标签是小程序中的一个视图容器，类似于HTML中的div标签，用于包裹和组织其他元素。
	class属性用于指定元素的样式类。
	Calendar组件是一个自定义组件，通过@dayChange和@monthChange事件监听器绑定了dayChange和monthChange方法。
	{{dayStr}}、{{weekStr}}、{{yearStr}}、{{yi}}和{{ji}}是Vue中的模板插值语法，用于显示数据。 -->
<!-- 这一页是展示选中的日期的，农历xxx，宜xxx忌xxx -->
<template>
	<view class="content">
		<Calendar  @dayChange='dayChange' @monthChange='monthChange' >
		</Calendar>
		<view class="bg-white padding " style="margin-top: 10rpx;">
			<view class="flex justify-between solid-bottom padding padding-bottom align-center">
				<view class="text-xl text-bold padding bg-blue shadow round light solid-right"><view class="text-red">农</view><view class="text-red">历</view></view>
				<view class="text-xxl text-bold padding text-shadow text-black">{{dayStr}}</view>
				<view class=" padding flex flex-direction align-center solid-left">
					<!-- 星期 -->
					<view class="text-bold text-df padding-bottom-xs">{{weekStr}}</view>
					<!-- 黄色的xxx年 -->
					<view class="text-yellow text-lg">{{yearStr}}</view>
				</view>
			</view>
			<view class="padding">
				<view class="flex align-center cu-card padding-bottom-xs">
					<view class="cu-tag round bg-green">宜</view>
					<!-- 宜的数据 -->
					<view class="text-gray text-content text-sm margin-left">{{yi}}</view>
				</view>
				<view class="flex align-center cu-card ">
					<view class="cu-tag round bg-red ">忌</view>
					<view class="text-gray text-content text-sm margin-left">{{ji}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<!-- 在脚本（script）部分，首先引入了一个名为Li-Calendar的自定义组件，然后导出了一个默认的Vue组件对象。 -->
<!-- 在data函数中，定义了组件的数据属性，包括selectDay（选中的日期）、dayStr（农历日期）、weekStr（星期几）、yearStr（农历年份）、yi（宜事项）和ji（忌事项）等。 -->
<!-- created钩子函数在组件实例创建完成后被调用，用于初始化数据。它获取当前日期，并将其格式化为字符串赋值给selectDay，然后调用render方法进行数据渲染。 -->
<script>
// import Calendar from '@/components/Li-Calendar/Li-Calendar.vue'; 导入名为Calendar的自定义组件。
// export default 将当前组件导出为默认的Vue组件。
	import Calendar from '@/components/Li-Calendar/Li-Calendar.vue';
	export default {
		components: {
			Calendar
		},
		data() {
			return {
				selectDay: '',
				dayStr:'',
				weekStr:'',
				yearStr:'',
				yi:'',
				ji:'',
			}
		},
		// created钩子函数在组件实例创建完成后被调用，用于初始化数据。它获取当前日期，并将其格式化为字符串赋值给selectDay，然后调用render方法进行数据渲染。
		created() {
			// 上面有定义data
			let curDate = new Date();
			this.selectDay =  curDate.getFullYear() + "/" + (curDate.getMonth() + 1) + "/" + curDate.getDate();
			this.render();
		},
		onLoad() {
	
		},
		// methods对象中定义了组件的方法，包括render方法用于渲染数据，
		// dayChange方法用于处理日期选择变更事件，monthChange方法用于处理月份选择变更事件，
		// 以及onShareAppMessage和onShareTimeline方法用于处理分享相关的事件。
		methods: {
			render(setDateStr) { 
				console.info("开始进行首页下数据渲染:"+setDateStr)
				let dd = setDateStr?new Date(setDateStr):new Date(this.selectDay);
				let d = Lunar.fromDate(dd);
				this.dayStr = d.getMonthInChinese() + '月' + d.getDayInChinese();
				// 农历的年表示方法：干支+生肖
				this.yearStr = d.getYearInGanZhi() + '(' + d.getYearShengXiao() + ')年';
				this.weekStr = '星期' + d.getWeekInChinese();
				this.ji = d.getDayJi().join(' ');
				this.yi = d.getDayYi().join(' ');
				
			},
			dayChange(data) {
				this.selectDay = data.time;
				this.render(data.time);
				console.log("日期选择变更",data)
			},
			monthChange(data) {
				console.log("月份选择变更",data)
			},
			// onShareAppMessage 和 onShareTimeline 方法分别用于处理分享到微信和分享到朋友圈的事件。它们返回一个包含分享标题、路径等信息的对象。
			onShareAppMessage(res) {
			    if (res.from === 'button') {// 来自页面内分享按钮
			      console.log(res.target)
			    }
			    return {
			      title: '假期日历',
			      path: '/pages/index/index',
				  mpId: 'wxee05508955140bfd'
			    }
			},
			onShareTimeline(res){
				  if (res.from === 'button') {// 来自页面内分享按钮
					console.log(res.target)
				  }
				  return {
					title: '假期日历',
					path: '/pages/index/index',
					mpId: 'wxee05508955140bfd'
				  }
			},
		}
	}
	// 在script标签的末尾，使用require语句引入了一个名为lunar.js的模块，并导入其中的Solar、SolarMonth、Lunar和HolidayUtil等对象。
	const {Solar,SolarMonth,Lunar,HolidayUtil} = require('@/common/lunar.js');
	
	
	
</script>

<style>
	
</style>
