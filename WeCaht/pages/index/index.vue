<template>
	<view class="home">
		<!-- banner -->
		<view class="banner">
			<swiper autoplay indicator-dots circular interval="3000" indicator-color="#333" indicator-active-color="#fff">
				<swiper-item v-for="(item,index) in banner" :key="index">
					<image :src="item.picurl" mode="aspectFill"></image>
				</swiper-item>
			</swiper>
		</view>
		<!-- banner end -->
	</view>
	
	<!-- notice -->
	<view class="notice">
		<view class="left">
			<uni-icons type="sound" size="20" color="#3cb371"></uni-icons>
			<!-- annonce -->
			<text class="annonce">公告</text>
		</view>
		<view class="middle">
			<swiper  autoplay interval="3000" circular vertical disable-touch>
				<swiper-item>
					<view class="swiper-item">五一劳动节连放5天假</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">欢迎关注wallpaper公众号</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">星光不问赶路人,时光不负有心人爱上卡掉了</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="right">
			<uni-icons type="forward" size="20" color="#3cb371"></uni-icons>
		</view>
	</view>
	<!-- notice end -->
	
	<!-- recommend -->
	<view class="recommend">
		<view class="title">
			<view class="left">
				<text>每日推荐</text>
			</view>
			<view class="right">
				<!-- icon -->
				<uni-icons type="calendar" size="30" color="#3CB371"></uni-icons>
				<text class="day" v-for="(item,index) in res" :key="index">{{item.year}}-{{item.month}}-{{item.day}}</text><!-- v-text="days" -->
			</view>
		</view>
		<view class="content">
			<scroll-view scroll-x="true" >
				<view class="item" v-for="(item,index) in Recommend" :key="index">
					<image :src="item.smallPicurl" mode="aspectFill"></image>
				</view>
				<view class="detail"></view>
			</scroll-view>
		</view>
	</view>
	<!-- recommend end -->

	<!-- container -->
	<view class="container">
		<view class="title">
			<view class="left">
				专题精选
			</view>
			<view class="right">
				More+
			</view>
			
		</view>
		<view class="content">
			<view class="item" v-for="item in containerList" @click="navclassify(item._id,item.name)" ><!-- 把item对应的id传递到navigateTo中，就能实现点击显示对应的id内容 -->
				<!-- {{item._id}} -->
				<image :src="item.picurl" mode="aspectFill"></image>
				<view class="item-name">{{item.name}}</view>
				<view class="timer">1天前更新</view>
			</view>
			<navigator url="/pages/classify/classify" open-type="switchTab" class="item more" @click="ToPage">
				<image src="/static/images/classify1.jpg" mode="aspectFill"></image>
				<view class="mores">
					<uni-icons type="more-filled" size="30" color="white"></uni-icons>
					<text>更多</text>
				</view>
			</navigator>
		</view>
	</view>
	<!-- container end -->

</template>

<script setup>
// 跳转
let navclassify = (cid,cname) => {
	uni.navigateTo({
		url:"/pages/classifylist/classifylist?id=" + cid + "&name=" + cname
	})
	// console.log(cid)
}	
// 使用接口获取数据
let banner = ref([])
// 专题精选
let containerList = ref([])
// 海报
let Recommend = ref([])
let getRecommend = async()=> {
	let result = await uni.request({
		url:"https://tea.qingnian8.com/api/bizhi/randomWall"
	})
	Recommend.value = result.data.data
	
}
getRecommend()
// 轮播图
let getBanner = async()=> {
	let result = await uni.request({
		url:"https://tea.qingnian8.com/api/bizhi/homeBanner"
	})
	banner.value = result.data.data

}
getBanner()
// 专题精选
let getContainer = async()=> {
	let result = await uni.request({
		url:"https://tea.qingnian8.com/api/bizhi/classify",
		data:{
			// 一页获取几条数据
			pageSize:8
		}
	})
	// console.log(result)
	containerList.value = result.data.data
}
getContainer()
import { reactive, ref } from 'vue';
// 获取年份
const year = ref(0)
const month = ref(0)
const day = ref(0)
let now = new Date()
year.value = now.getFullYear()
month.value = now.getMonth() + 1
day.value = now.getDate()
// 存放数据
const res = [{
	year,
	month,
	day
}]
console.log(res)
// console.log(days._rawValue)
</script>

<style lang="scss" scoped>
	// banner
	.banner {
		width: 750rpx;
		padding: 30rpx 0;
		swiper {
			width: 750rpx;
			height: 340rpx;
			swiper-item {
				width: 100%;
				height: 100%;
				padding: 0 30rpx;
				image {
					width: 100%;
					height: 100%;
					border-radius: 15rpx;
			}
		}
	}	
}
.notice {
	width: 690rpx;
	height: 80rpx;
	margin: 0 auto;
	display: flex;
	align-items: center;
	background-color: lightyellow;
	.left {
		width: 120rpx;
		display: flex;
		.annonce {
			color: #3cd371;
			font-weight: bold;
		}
	}
	.right {
		width: 30rpx;
	}
	.middle {
		flex: 1;
		height: 80rpx;
		// 设置轮播字幕剧中
		line-height: 80rpx;
		swiper {
			height: 100%;
			swiper-item {
				height: 100%;
				.swiper-item {
					height: 100%;
					// padding-left: 0rpx;
					// 设置文字超出显示省略号
					text-overflow: ellipsis;
					white-space: nowrap;
					overflow: hidden;
				}
			}
		}
	}
}
// banner end

// recomment
.recommend {
	padding: 30rpx;
	.title {
		height: 80rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: lightyellow;
		.left {
			text {
				font-weight: bold;
			}
		}
		.right {
			display: flex;
			align-items: center;
		}
	}
	.content {
		margin-top: 20rpx;
		width: 720rpx;
		scroll-view {
			white-space: nowrap;
			height: 440rpx;
			
			// 隐藏滑动条
			::-webkit-scrollbar {
				display: none;
			}
			.item{
				width: 200rpx;
				height: 430rpx;
				// position: relative;
				// view为块级元素，要转换成行内元素，如果不转换，则white-space没有效果
				display: inline-block;
				margin-right: 15rpx;
				image {
					// 设置图片大小与容器大小相同，否则图片会超出容器显示（排版出现问题）
					width: 100%;
					height: 100%;
					border-radius: 15rpx;
				}
			}
			// 选中item元素中的最后一个，进行外边距设置排版，与全部的内边距进行对应，padding30
			.item:last-child {
				// 不设置内边距会出现最后一个元素超出布局一些
				margin-right: 30rpx;
			}
		}
	}
}
// recomment end

// container
.container {
	padding: 30rpx;
	.title {
		height: 80rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: lightyellow;
		.left {
			text {
				font-weight: bold;
			}
		}
		.right {
			display: flex;
			align-items: center;
		}
	}
	.content {
				margin-top: 20rpx;
				// grid布局
				display: grid;
				grid-template-columns: repeat(3, 1fr);
				// gap: 10rpx;
				row-gap: 10rpx;
				.item {
					width: 220rpx;
					height: 340rpx;
					// 相对定位
					position: relative;
					image {
						width: 100%;
						height: 100%;
						border-radius: 15rpx;
					}
					.item-name {
						position: absolute;
						bottom: 0;
						left: 0;
						background-color: rgba(0, 0, 0, 0.2);
						height: 70rpx;
						width: 100%;
						text-align: center;
						line-height: 70rpx;
						color: #fff;
						font-weight: bold;
						border-radius: 0 0 15rpx 15rpx;
						// 磨砂
						backdrop-filter: blur(20rpx);
					}
					
					.timer {
						position: absolute;
						top: 0;
						left: 0;
						width: 60%;
						height: 50rpx;
						background-color: rgba(0, 0, 0, 0.2);
						font-size: 24rpx;
						text-align: center;
						line-height: 50rpx;
						color: #fff;
						backdrop-filter: blur(20rpx);
						border-radius: 15rpx 0 0 0;
					}
				}
			}
			.more {
				.mores {
					width: 100%;
					height: 100%;
					position: absolute;
					background-color: rgba(0, 0, 0, 0.2);
					backdrop-filter: blur(20rpx);
					top: 0;
					left: 0;
					border-radius: 15rpx;
					display: flex;
					justify-content: center;
					align-items: center;
					flex-direction: column;
					text {
						color: white;
						font-size: bold;
					}
				}
			}
		
}
// container end

</style>
