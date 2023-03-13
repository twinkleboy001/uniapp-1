<template>
	<view>
		<view class="flex align-center justify-between px-2">
			<text class="font-md">热门分类</text>
			<view @click="openMore" class="flex align-center font text-secondary">
				更多 <text class="iconfont icon-jinru"></text>
			</view>
		</view>
		<view class="flex align-center pb-3 px-2 border-bottom flex-wrap">
			<view v-for="(item, index) in hotCate" :key="index" class="border rounded bg-light px-2 mx-1 mt-3 animated" hover-class="jello">{{item.name}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: ['hotCate'],
		methods: {
			openMore() {
				console.log('openMore')
			},
			openDetail(item) {
				console.log('打开话题分类页')
			}
		}
	}
</script>

<style>
</style>