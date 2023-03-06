<template>
	<view>
		<block v-for="(item, index) in list" :key="index">
			<!-- 列表样式 -->
			<common-list :item="item" :index="index" @follow="follow" @doSupport="doSupport"></common-list>
			<!-- 全局分割线 -->
			<divider></divider>
		</block>
	</view>
</template>

<script>
	import CommonList from '@/components/common/common-list.vue';
	export default {
		components: {
			CommonList
		},
		data() {
			return {
				list: [
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
							unsupport_count: 2,
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
							support_count: 10,
							unsupport_count: 2,
						},
						comment_count: 2,
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
							support_count: 8,
							unsupport_count: 3,
						},
						comment_count: 2,
						share_num: 6
					}
				]
			}
		},
		onLoad() {

		},
		methods: {
			follow(e) {
				this.list[e].isFollow = true;
				uni.showToast({
					title: '关注成功'
				})
			},
			doSupport(e) {
				this.list[e.index].support.type = e.type;
			}
		}
	}
</script>

<style>
	
</style>
