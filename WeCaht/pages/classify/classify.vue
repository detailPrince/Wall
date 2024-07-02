<template>
	<view>
		<view class="wrap">
			<view class="classify">
				<navigator url="/pages/preview/preview"   class="item" v-for="(item,index) in 12" :key="indx">
					<image src="../../static/images/classify1.jpg" mode=""></image>
					<view class="item-name">明星美女</view>
					<view class="timer">1天前更新</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script setup>

</script>

<style lang="scss" scoped>
.wrap {
	.classify {
		padding: 30rpx;
		display: grid;
		grid-template-columns: repeat(3,1fr);
		gap: 10rpx;
		.item {
			position: relative;
			width: 220rpx;
			height: 340rpx;
			border-radius: 15rpx;
			overflow: hidden;
			image {
				width: 100%;
				height: 100%;
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
}
</style>
