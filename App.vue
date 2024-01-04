<script>
	import Vue from 'vue'
	export default {
		onLaunch: function() {
			uni.getSystemInfo({
				success: function(e) {
					// #ifndef MP
					Vue.prototype.StatusBar = e.statusBarHeight;
					if (e.platform == 'android') {
						Vue.prototype.CustomBar = e.statusBarHeight + 50;
					} else {
						Vue.prototype.CustomBar = e.statusBarHeight + 45;
					};
					// #endif

					// 首先将e.statusBarHeight的值赋给Vue.prototype.StatusBar。然后，使用微信小程序提供的wx.getMenuButtonBoundingClientRect()方法获取菜单按钮的位置信息，并将其赋给custom变量。最后，计算Vue.prototype.CustomBar的值为custom.bottom + custom.top - e.statusBarHeight。这些属性可以在Vue组件中用于自定义导航栏的布局。
					// #ifdef MP-WEIXIN
					Vue.prototype.StatusBar = e.statusBarHeight;
					let custom = wx.getMenuButtonBoundingClientRect();
					Vue.prototype.Custom = custom;
					Vue.prototype.CustomBar = custom.bottom + custom.top - e.statusBarHeight;
					// #endif		

					// 上述代码处理支付宝小程序的情况。它将e.statusBarHeight与e.titleBarHeight相加，并将结果赋给Vue.prototype.CustomBar。同时，将e.statusBarHeight的值赋给Vue.prototype.StatusBar。这些属性可以在Vue组件中用于自定义导航栏的布局。
					// #ifdef MP-ALIPAY
					Vue.prototype.StatusBar = e.statusBarHeight;
					Vue.prototype.CustomBar = e.statusBarHeight + e.titleBarHeight;
					// #endif
				}
			})

			Vue.prototype.ColorList = [{
					title: '嫣红',
					name: 'red',
					color: '#e54d42'
				},
				{
					title: '桔橙',
					name: 'orange',
					color: '#f37b1d'
				},
				{
					title: '明黄',
					name: 'yellow',
					color: '#fbbd08'
				},
				{
					title: '橄榄',
					name: 'olive',
					color: '#8dc63f'
				},
				{
					title: '森绿',
					name: 'green',
					color: '#39b54a'
				},
				{
					title: '天青',
					name: 'cyan',
					color: '#1cbbb4'
				},
				{
					title: '海蓝',
					name: 'blue',
					color: '#0081ff'
				},
				{
					title: '姹紫',
					name: 'purple',
					color: '#6739b6'
				},
				{
					title: '木槿',
					name: 'mauve',
					color: '#9c26b0'
				},
				{
					title: '桃粉',
					name: 'pink',
					color: '#e03997'
				},
				{
					title: '棕褐',
					name: 'brown',
					color: '#a5673f'
				},
				{
					title: '玄灰',
					name: 'grey',
					color: '#8799a3'
				},
				{
					title: '草灰',
					name: 'gray',
					color: '#aaaaaa'
				},
				{
					title: '墨黑',
					name: 'black',
					color: '#333333'
				},
				{
					title: '雅白',
					name: 'white',
					color: '#ffffff'
				},
			]
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
	// 创建了一个对象o，其中包含了各个日期和时间的格式化符号
	Date.prototype.format = function(fmt) {
		var o = {
			"M+": this.getMonth() + 1, //月份 
			"d+": this.getDate(), //日 
			"h+": this.getHours(), //小时 
			"m+": this.getMinutes(), //分 
			"s+": this.getSeconds(), //秒 
			"q+": Math.floor((this.getMonth() + 3) / 3), //季度 
			"S": this.getMilliseconds() //毫秒 
		};
		// 这部分代码处理年份的格式化，使用正则表达式匹配字符串fmt中的年份符号（连续的y），并将其替换为对应的年份。例如，如果fmt为"yyyy-MM-dd"，则会将"yyyy"替换为实际的年份
		if (/(y+)/.test(fmt)) {
			fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
		}
		// 这部分代码遍历对象o的属性，使用正则表达式匹配字符串fmt中对应的符号，并将其替换为实际的日期或时间值。如果符号对应的值是个位数，则在前面补零。例如，如果fmt为"yyyy-MM-dd hh:mm:ss"，则会将"yyyy"替换为实际的年份，"MM"替换为实际的月份，以此类推。
		for (var k in o) {
			if (new RegExp("(" + k + ")").test(fmt)) {
				fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k])
					.length)));
			}
		}
		return fmt;
	}
</script>

<style>
	/*每个页面公共css */
	@import "common/colorui/main.css";
	@import "common/colorui/icon.css";
</style>
