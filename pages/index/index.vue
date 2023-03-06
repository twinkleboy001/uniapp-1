<template>
	<view>
		<scroll-view scroll-x :scroll-into-view="scrollInto" scroll-with-animation class="scroll-row border-bottom border-light-secondary">
			<view v-for="(item, index) in tabBars" class="scroll-row-item px-3 py-2 font-md" :key="index" :id="'tab' + index" :class="tabIndex === index ? 'text-main font-lg font-weight-bold' : ''" @click="changeTab(index)">{{item.name}}</view>
		</scroll-view>
		
		<!-- <block v-for="(item, index) in list" :key="index"> -->
			<!-- 列表样式 -->
			<!-- <common-list :item="item" :index="index" @follow="follow" @doSupport="doSupport"></common-list> -->
			<!-- 全局分割线 -->
			<!-- <divider></divider> -->
		<!-- </block> -->
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
				scrollInto: "",
				tabIndex: 0,
				tabBars: [{name: '关注'},{name: '推荐'},{name: '体育'},{name: '热点'},{name: '财经'},{name: '娱乐'},{name: '军事'},{name: '历史'},{name: '本地'}],
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
				let item = this.list[e.index];
				let msg = e.type === 'support' ? '顶' : '踩'
				if(item.support.type === '') {
					item.support[e.type+'_count']++;
				} else if(item.support.type === 'support' && e.type === 'unsupport') {
					item.support.support_count--;
					item.support.unsupport_count++;
				} else if(item.support.type === 'unsupport' && e.type === 'support') {
					item.support.support_count++;
					item.support.unsupport_count--;
				}
				item.support.type = e.type;
				uni.showToast({
					title: msg + '成功'
				})
			},
			changeTab(index) {
				if(this.tabIndex === index) {
					return;
				}
				this.tabIndex = index;
				this.scrollInto = 'tab' + index;
			}
		}
	}
</script>

<style>
	
</style>
