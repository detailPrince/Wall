<template>
<view class="home">
	<!-- banner -->
	<view class="banner">
		<swiper 
		indicator-dots="#666" 
		indicator-active-color="#fff"
		autoplay
		interval="3000"
		circular
		>
			<swiper-item v-for="item in 3">
				<image src="/static/images/banner2.jpg" mode=""></image>
			</swiper-item>
		</swiper>
	</view>
	<!-- end banner -->
	<!-- notice -->
	<view class="notice">
		<view class="left">
			<uni-icons type="sound-filled" size="20" color="#3CB371"></uni-icons>
			<!-- annonce -->
			<text class="annonce">公告</text>
			
		</view>
		<view class="middle">
			<swiper vertical interval="3000" autoplay circular>
				<swiper-item>
					<view class="swiper-item">五一劳动节连放5天假</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">欢迎关注wallpaper公众号</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">星光不问赶路人,时光不负有心人</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">Gitee拥有全面的开源项目分类和推荐，帮助你迅速找到有价值的开源项目并参与其中。现在就创建仓库或探索 Gitee，开启你的开源之旅。</view>
				</swiper-item>
				
			</swiper>
		</view>
		<view class="right">
			<uni-icons type="right" size="20" color="#3CB371"></uni-icons>
		</view>
	</view>
	<!-- end notice -->
	<!-- recommend -->
	<view class="recommend">
		<view class="title">
			<view class="left">每日推荐</view>
			<view class="right">
				<uni-icons type="calendar" size="20" color="#3cb371"></uni-icons>
				<text v-text="timers"></text>
			</view>
		</view>
		<view class="content">
			<scroll-view scroll-x >
				<view class="item" v-for="(item, index) in recList" :key="index">
					<image :src="item.smallPicurl" mode="aspectFill"></image>
				</view>
			</scroll-view>
		</view>
	</view>
	<!-- end recommend -->
	<!-- selected -->
	<view class="selected">
		<view class="title">
			<view class="left">专题精选</view>
			<view class="right">More+</view>
		</view>
		<view class="content">
			<view class="item" v-for="item in 8" @click="clickChange">
				<image src="/static/images/classify1.jpg" mode="aspectFill"></image>
				<view class="item-name">明星美女</view>
				<view class="timer">1天前更新</view>
			</view>
			<navigator url="/pages/classify/classify" open-type="switchTab" class="item more">
				<image src="/static/images/classify1.jpg" mode="aspectFill"></image>
				<view class="mores">
					<uni-icons type="more-filled" size="30" color="#fff"></uni-icons>
					<text>更多</text>
				</view>
			</navigator>
		</view>
	</view>
	<!-- end selected -->
</view>
</template>

<script setup>
import { ref } from 'vue';
let timer = new Date()
let year = timer.getFullYear()
let month = timer.getMonth() + 1
let day = timer.getDate()
let timers = year + '-' + month + '-' + day

// 推荐的响应式数据
let recList = ref([])

let clickChange  = ()=>{
	uni.navigateTo({
		url:'/pages/classifylist/classifylist'
	})
}

// 每日推荐
let getRecommend =async ()=>{
	// 发起请求
	let result =await uni.request({
		url:'https://tea.qingnian8.com/api/bizhi/randomWall'
	})
	recList.value = result.data.data
}

// 使用
getRecommend()
</script>

<style lang="scss" scoped>
.home {
	.banner {
		// background-color: pink;
		width: 750rpx;
		padding-top: 30rpx;
		padding-bottom: 30rpx;
		swiper {
			width: 750rpx;
			height: 340rpx;
			padding-left: 30rpx;
			padding-right: 30rpx;
			swiper-item {
				// 宽度高度跟swiper保持一致
				width: 100%;
				height: 100%;
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
		background-color: lightyellow;
		margin: 0 auto;
		border-radius: 40rpx;
		// flex布局
		display: flex;
		// justify-content: space-between;
		// 垂直方向居中
		align-items: center;
		.left {
			width: 120rpx;
			.annonce {
				color: #3CB371;
				font-weight: bold;
			}
		}
		.right {
			width: 40rpx;
		}
		.middle {
			// 把剩余空间全部分配给中间
			flex: 1;
			height: 80rpx;
			line-height: 80rpx;
			swiper {
				height: 100%;
				swiper-item{
					height: 100%;
					view {
						height: 100%;
						// 省略号
						overflow: hidden;
						white-space: nowrap;
						text-overflow: ellipsis;
					}
				}
			}
		}
	}
	
	.recommend {
		padding: 30rpx;
		.title {
			height: 80rpx;
			background-color: lightyellow;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.left {
				font-size: 40rpx;
			}
			.right {
				color: #3CB371;
			}
		}
		.content {
			margin-top: 20rpx;
			width: 720rpx;
			scroll-view{
				height: 440rpx;
				white-space: nowrap;
				.item {
					width: 200rpx;
					height: 430rpx;
					display: inline-block;
					margin-right: 15rpx;
					image {
						width: 100%;
						height: 100%;
						border-radius: 15rpx;
					}
					&:last-child {
						margin-right: 30rpx;
					}
				}
			}
		}
	}
	
	.selected {
		padding: 30rpx;
		.title {
			height: 80rpx;
			background-color: lightyellow;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.left {
				font-size: 40rpx;
			}
			.right {
				color: #3CB371;
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
			.more {
				.mores{
					position: absolute;
					top: 0;
					left: 0;
					width: 100%;
					height: 100%;
					background-color: rgba(0, 0, 0, 0.2);
					backdrop-filter: blur(20rpx);
					display: flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					border-radius: 15rpx;
					text {
						color: #fff;
						font-weight: bold;
					}
				}
			}
		}
	}
}
</style>
