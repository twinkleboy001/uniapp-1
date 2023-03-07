<template>
	<view>
		<template v-if="searchList.length === 0">
			<view class="py-2 font-md px-2">搜索历史</view>
			<view class="flex flex-wrap">
				<view class="border rounded font mx-2 my-1 px-2" v-for="(item, index) in list" :key="index" hover-class="bg-light" @click="clickSearchHistory(item)">{{ item }}</view>
			</view>
		</template>
		<template v-else>
			<!-- 数据列表 -->
			<block v-for="(item, index) in searchList" :key="index">
				<common-list :item="item" :index="index"></common-list>
			</block>
		</template>
	</view>
</template>

<script>
	// 测试数据
	const demo = [
		{
			username: "昵称1",
			userPic: "/static/default.jpg",
			newstime: "2019-10-20 上午09:35",
			isFollow: false,
			title: "我是标题",
			titlePic: "/static/demo/demo11.jpg",
			support: {
				type: "support",
				support_count: 10,
				unsupport_count: 0,
			},
			comment_count: 2,
			share_num: 6
		},
		{
			username: "昵称2",
			userPic: "/static/default.jpg",
			newstime: "2019-10-20 下午04:30",
			isFollow: false,
			title: "我是标题2",
			titlePic: "",
			support: {
				type: "unsupport",
				support_count: 3,
				unsupport_count: 10,
			},
			comment_count: 0,
			share_num: 6
		},
		{
			username: "昵称3",
			userPic: "/static/default.jpg",
			newstime: "2019-10-22 下午02:16",
			isFollow: false,
			title: "我是标题3",
			titlePic: "",
			support: {
				type: "",
				support_count: 0,
				unsupport_count: 3,
			},
			comment_count: 2,
			share_num: 0
		}
	]
	import CommonList from '@/components/common/common-list.vue'
	export default {
		components: {
			CommonList
		},
		data() {
			return {
				searchText: "",
				list: ['uni-app前端开发', 'react hook', 'vue3.0', 'Java后端架构师'],
				searchList: []
			}
		},
		onNavigationBarSearchInputChanged(e) {
			this.searchText = e.text;
		},
		onNavigationBarButtonTap(e) {
			if(e.index === 0) {
				this.searchEvent()
			}
		},
		methods: {
			// 点击搜索历史
			clickSearchHistory(text) {
				this.searchText = text
				this.searchEvent()
			},
			searchEvent() {
				// 收起键盘
				uni.hideKeyboard()
				uni.showLoading({
					title:'加载中...',
					mask:true
				})
				setTimeout(()=>{
					this.searchList = demo;
					uni.hideLoading()
				}, 2000);
			}
		}
	}
</script>

<style>

</style>
