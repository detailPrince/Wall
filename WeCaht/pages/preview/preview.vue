<template>
	<view class="box">
		<swiper circular :current=" imgId" @change="swiperChange">
			<swiper-item v-for="(item,index) in preViewList">
				<view class="swiperbox" @click="changeMark"> <!-- 蒙版点击事件 -->
				<image :src="item.bigimg" mode="aspectFill"></image>
				</view>
			</swiper-item>
		</swiper>
		<!-- mask -->
				<view class="mask" v-if="status">
					<view class="top">
						<!-- count -->
						<view class="count">{{imgId + 1}}/{{preViewList.length}}</view>
						<!-- timer -->
						<view class="timer">
							<uni-dateformat :date="new Date()" format="hh:mm"></uni-dateformat>
						</view>
						<!-- dater -->
						<view class="dater">
							<uni-dateformat :date="new Date()" format="MM月dd日"></uni-dateformat>
						</view>
					</view>
					
					<view class="bottom">
						<view class="sub" @click="popinfo">
							<!-- 图标 -->
							<uni-icons type="info" size="20"></uni-icons>
							<text>信息</text>
						</view>
						<view class="sub" @click="rateinfo">
							<!-- 图标 -->
							<uni-icons type="star"></uni-icons>
							<text>{{rateValue}}分</text>
						</view>
						<view class="sub">
							<!-- 图标 -->
							<uni-icons type="download"></uni-icons>
							<text>下载</text>
						</view>
					</view>
				</view>
		
		<!-- 弹层 -->
	<uni-popup ref="popupinfo" type="bottom">
				<view class="popupinfo">
					<view class="title">
						<view class="left"></view>
						<view class="center">wallpaper信息</view>
						<view class="close" @click="closeinfo">
							<uni-icons type="closeempty" size="20"></uni-icons>
						</view>
					</view>
					<view class="content">
						<scroll-view scroll-y="true">					
							<!-- id -->
							<view class="item">
								<label>壁纸ID：</label>
								<view>123456abc</view>
							</view>
							<!-- classifyinfo -->
							<view class="item">
								<label>分类：</label>
								<view>明星</view>
							</view>
							<!-- author -->
							<view class="item">
								<label>发布者：</label>
								<view>皮皮虾</view>
							</view>
							<!-- rate -->
							<view class="item">
								<label for="">评分：</label>
								<view class="rate">
									<uni-rate  value="4.5"  allowHalf/>
								</view>
							</view>
						</scroll-view>
					</view>
				</view>
			</uni-popup>
			
		<!-- rate -->
		<uni-popup ref="ratepopup" :is-mask-click="false">
			<view class="ratepopup">
				<view class="title">
					<view class="left"></view>
					<view class="center">信息</view>
					<view class="right" @click="rateclose">
						<uni-icons type="closeempty"></uni-icons>
					</view>
				</view>
				<view class="content">
					<view class="rate">
						<uni-rate v-model ="rateValue" allowHalf />
						<text>{{rateValue}}分</text>
					</view>
					<view class="confirm">
						<button :disabled="rateValue!=0?false:true" @click="confirmRate" type="default" size="mini" plain>确认评分</button>
					</view>
				</view>
			</view>
		</uni-popup>
		<!-- rate end -->
	</view>
</template>

<script setup>
import { ref } from 'vue';
// 导入onLoad
import { onLoad } from '@dcloudio/uni-app'


let imgId = ref(null)

// 获取classifylist中对应的id(实现点击后显示对应的photo)
onLoad((e)=> {
	imgId.value = parseInt(e.index)
})

// 获取swiper中的事件
let swiperChange = (e)=> {
	 imgId.value = e.detail.current
}

// 保存读取到localstorage中的数据
let preViewList = ref([])

// 读取localStorage中的数据
let resultLists = uni.getStorageSync('storages');
// 返回新的数组
preViewList.value = resultLists.map(item=> {
	return {
		...item,
		// 添加新属性（把后缀改变后的大图片）
		'bigimg':item.smallPicurl.replace('_small.webp','.jpg')
	}
})
console.log(preViewList.value)
// 评分
let rateValue = ref(0)
// 定义遮罩是否显示的状态
let status = ref(true)
let popupinfo = ref(null)
// 确认评分显示分数
let score = ref(false)
// 评分弹层
let ratepopup = ref(null)
let rateinfo = () => {
	ratepopup.value.open()
}
let rateclose = () => {
	ratepopup.value.close()
}
// 确认评分
let confirmRate = () => {
	score.value = true
	alert("评分成功!")
	rateclose()
}
// 评分弹层 end
// 改变状态status
let maskStatus = ()=>{
	status.value = !status.value
	console.log(status.value)
}

let popinfo = ()=>{
	// 打开弹出框
	popupinfo.value.open()
	// alert('hello')
}

// 关闭弹出信息
let closeinfo = ()=>{
	popupinfo.value.close()
}

// 时间
const hours = ref(0)
const minute = ref(0)
const Month = ref(0)
const day = ref(0)
let now = new Date()
hours.value = now.getHours()
minute.value = now.getMinutes()
Month.value = now.getMonth() + 1
day.value = now.getDate()
let Times = [{
	hours:hours,
	minute:minute,
	Month:Month,
	day:day
}]
</script>

<style lang="scss" scoped>
.box {
	swiper {
		position: relative;
		width: 100%;
		height: 100vh;
		swiper-item {
			width: 100%;
			height: 100%;
			position: relative;
			.swiperbox {
				width: 100%;
				height: 100%;
				image {
					width: 100%;
					height: 100%;
		}
			}
			
		
	  }
}
	.mask{
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.1);
		position: absolute;
		top: 0;
		left: 0;
		// 取消遮罩对事件的影响
		pointer-events: none;
		.top {
			margin-top: 100rpx;
			display: flex;
			// 改变主轴的方向为y
			flex-direction: column;
			// 垂直居中
			justify-content: center;
			// 水平居中
			align-items: center;
			font-size: 30rpx;
			color: #fff;
			.timer {
				font-size: 100rpx;
				margin-top: 30rpx;
				margin-bottom: 15rpx;
			}
		}
		.bottom {
			// 正常
			pointer-events: auto;
			// 屏幕宽度的60%
			width: 60vw;
			height: 80rpx;
			background-color: #f0f0f0;
			border-radius: 40rpx;
			margin: 0 auto;
			margin-top: 900rpx;
			display: flex;
			justify-content: space-around;
			align-items: center;
			.sub {
				display: flex;
				flex-direction: column;
				font-size: 24rpx;
			}
		}
	}

	.popupinfo {
		width: 100%;
		background-color: #fff;
		border-radius: 30rpx 30rpx  0 0;
		.title {
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 80rpx;
			border-bottom: 1px solid #ccc;
		}
		.content {
			padding: 30rpx;
			scroll-view {
				width: 100%;
				// 最大高度是60%
				max-height: 50vh;
				.item {
					display: flex;
				}
			}
		}
	}
}
// rate
.ratepopup	 {
	width: 70vw;
	background-color: #fff;
	padding: 0 10rpx;
	border-radius: 15rpx;
	.title {
		display: flex;
		justify-content: space-between;
		height: 60rpx;
		align-items: center;
	}
	.content {
		.rate {
			display: flex;
			padding: 50rpx;
			justify-content: center;
		}
		.confirm {
			text-align: center;
		}
	}
}

</style>
