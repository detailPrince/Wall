<template>
	<view class="box">
		<view class="loading" v-if="!classify.length">
			<uni-load-more status="loading"></uni-load-more>
		</view>
		<view class="classifylist">
			<navigator :url="'/pages/preview/preview?index='+ index" class="item" v-for="(item,index) in classify">
				<image :src="item.smallPicurl" mode="aspectFill"></image>
			</navigator>
		</view>
	</view>
	<!-- 下拉加载更多 -->
	<view class="loading" v-if="classify.length">
		<uni-load-more :status="onData?'loading':'noMore'"></uni-load-more>
	</view>
</template>

<script setup>
import { ref } from 'vue';
import { onLoad,onReachBottom } from '@dcloudio/uni-app'
// 下拉加载
let onData = ref(true)


// 生命周期函数用对象接收
let paras = {
	pageNum:1
}
// 生命周期函数（语法糖的原因最后执行） 用于获取首页中对应的id
onLoad((e) => {
	paras.cid = e.id
	paras.cname = e.name
	// 获取对应的tilte
	uni.setNavigationBarTitle({
		title: paras.cname
	})
	getclassify()
})
// 触底触发事件
onReachBottom(()=> {
	getclassify()
})

// 分类列表
let classify = ref([])
let getclassify = async() => {
	let result = await uni.request({
		url:"https://tea.qingnian8.com/api/bizhi/wallList",
		// 获取分类id，根据id进行获取对应的分类壁纸
		data:{
			classid:paras.cid,
			// 触底加载
			pageNum:paras.pageNum++
		},
	})

	// 将加载出来的数据装入到数组中---实现不丢失前面的数据
	// console.log(result)
	classify.value = [...classify.value,...result.data.data]
	// 存储到本地缓存
	uni.setStorageSync('storages', classify.value)
// 判断显示下拉刷新
	if(result.data.data.length < 12) {
		onData.value = false
	}
}	

</script>

<style lang="scss" scoped>
.box {
	padding: 10rpx;
	.classifylist {
		display: grid;
		grid-template-columns: repeat(3,1fr);
		gap: 10rpx;
		.item {
			height: 440rpx;
			width: 100%;
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
}
</style>
