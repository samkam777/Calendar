<template>
	<view class="compass-page">
		<view class="compass-container">
		  <!-- <image
			class="compass-img"
			:src="compassSrc"
			mode="aspectFit"
		  ></image> -->
		  <image 
			class="compass-img" 
			:src="compassSrc"
			mode="aspectFit"
			:style="'transform: rotate(' + compassDegree + 'deg)'"></image>
		  <view class="compass-info">
			<text class="info-text">方向：{{ currentDirection }} </text>
			<text class="info-text">{{ compassDegree }}   °</text>
			<text class="info-text">   时间：{{ currentTime }}</text>
			<!-- <text class="info-text">图像路径：{{ compassSrc }}</text> -->
		  </view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				compassSrc: "/static/compass/compass2_cropping.png", // 风水罗盘图片路径
				currentDirection: '', // 当前方向
				currentTime: '', // 当前时间
				compassDegree: 0, // 当前具体度数
			};
		},
		// 监听页面加载事件
		  onLoad: function ()  {
		    // 在页面加载时开始监听方向变化
		    this.startCompass();
		  },
		
		  // 监听页面卸载事件
		  onUnload: function() {
		    // 在页面卸载时停止监听方向变化
		    this.stopCompass();
		  },
		
		mounted() {
		    // 获取当前设备方向
		    uni.onCompassChange((res) => {
		      this.currentDirection = this.getDirection(res.direction);
		    });
		
		    // 获取当前时间
		    this.currentTime = new Date().format('hh:mm:ss');
		  },
		methods: {
			startCompass() {
			      var that = this;
			      wx.startCompass({
			        success() {
			          wx.onCompassChange(function (res) {
			            var degree = res.direction.toFixed(2);
			            var rotation = 360 - parseFloat(degree);
			
			            that.compassDegree = Math.round(rotation);
			          });
			        },
			      });
			    },
			    stopCompass() {
			      wx.stopCompass();
			    },
			getDirection(direction) {
			      // 根据方向角度判断当前方向
			      if (direction >= 337.5 || direction < 22.5) {
			        return '北';
			      } else if (direction >= 22.5 && direction < 67.5) {
			        return '东北';
			      } else if (direction >= 67.5 && direction < 112.5) {
			        return '东';
			      } else if (direction >= 112.5 && direction < 157.5) {
			        return '东南';
			      } else if (direction >= 157.5 && direction < 202.5) {
			        return '南';
			      } else if (direction >= 202.5 && direction < 247.5) {
			        return '西南';
			      } else if (direction >= 247.5 && direction < 292.5) {
			        return '西';
			      } else if (direction >= 292.5 && direction < 337.5) {
			        return '西北';
			      }
			    },
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
		}
	}
</script>

<style>
.compass-page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f5f5f5;
}

.compass-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.compass-img {
  width: 395px;
  height: 395px;
}

.compass-info {
  margin-top: 20px;
  text-align: center;
}

.info-text {
  font-size: 16px;
  color: #333;
}
</style>
