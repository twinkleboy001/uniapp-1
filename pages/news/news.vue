<template>
	<view>
		<uni-nav-bar :border="false" :fixed="true" :statusBar="true" @clickRight="openAddInput">
			<view class="flex align-center justify-center font-weight-bold w-100">
				<view 
					class="mx-1" 
					v-for="(item, index) in tabBars" 
					:key="index"
					:class="tabIndex === index ? 'font-lg text-main' : 'font-md text-light-muted'"
					@click="changeTab(index)"
				>
					{{ item.name }}
				</view>
			</view>
			<text slot="right" class="iconfont icon-fatie_icon"></text>
		</uni-nav-bar>
		
		<swiper @change="onChangeTab" :current="tabIndex" :duration="150" :style="'height:' + scrollH + 'px'">
			<!-- 关注 -->
			<swiper-item>
				<scroll-view scroll-y :style="'height:' + scrollH + 'px'" @scrolltolower="loadmoreEvent">
					<block v-for="(item, index) in list" :key="index">
						<common-list :item="item" :index="index" @doSupport="doSupport"></common-list>
						<divider></divider>
					</block>
					<load-more :loadmore="loadmore"></load-more>
				</scroll-view>
			</swiper-item>
			<!-- 话题 -->
			<swiper-item>
				<scroll-view scroll-y :style="'height:' + scrollH + 'px'">
					<hot-cate :hotCate="hotCate"></hot-cate>
					<view class="p-2">
						<view class="bg-light rounded flex align-center justify-center py-2 text-secondary">
							<text class="iconfont icon-sousuo mr-2"></text>
							搜索话题
						</view>
					</view>
					<swiper class="px-2 pb-2" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
						<swiper-item>
							<image src="/static/demo/banner2.jpg" style="height: 300rpx;" class="w-100 rounded"></image>
						</swiper-item>
					</swiper>
					<divider></divider>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	const demo = [
		{
			username: "昵称1",
			userPic: "/static/default.jpg",
			newstime: "2019-10-20 上午09:35",
			isFollow: true,
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
			isFollow: true,
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
			isFollow: true,
			title: "我是标题3",
			titlePic: "",
			support: {
				type: "",
				support_count: 0,
				unsupport_count: 3,
			},
			comment_count: 2,
			share_num: 0
		},
		{
			username: "昵称4",
			userPic: "/static/default.jpg",
			newstime: "2019-10-20 上午09:35",
			isFollow: true,
			title: "我是标题4",
			titlePic: "/static/demo/demo10.jpg",
			support: {
				type: "support",
				support_count: 10,
				unsupport_count: 0,
			},
			comment_count: 2,
			share_num: 0
		},
	]
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	import commonList from '@/components/common/common-list.vue';
	import loadMore from '@/components/common/load-more.vue';
	import hotCate from '@/components/news/hot-cate.vue';
	export default {
		components: {
			uniNavBar,
			commonList,
			loadMore,
			hotCate
		},
		data() {
			return {
				scrollH: 500,
				tabIndex: 0,
				tabBars: [{name: '关注'}, {name: '话题'}],
				list: [],
				// 1.上拉加载更多 2.加载中... 3.没有更多了
				loadmore: "上拉加载更多",
				hotCate: [{name: "关注"}, {name: "推荐"},{name: "体育"},{name: "热点"},{name: "财经"}]
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success: (res) => {
					this.scrollH = res.windowHeight - res.statusBarHeight - 44
				}
			})
			this.list = demo
		},
		methods: {
			openAddInput() {
				console.log('add input')
				uni.navigateTo({
					url: '/pages/add-input/add-input'
				})
			},
			// 切换选项卡
			changeTab(index) {
				this.tabIndex = index
			},
			// 滑动
			onChangeTab(e) {
				this.tabIndex = e.detail.current
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
			loadmoreEvent() {
				// 验证当前是否处于可加载状态
				if(this.loadmore !== '上拉加载更多') return;
				// 设置加载状态
				this.loadmore = '加载中...';
				// 模拟请求数据
				setTimeout(() => {
					// 加载数据
					this.list = [...this.list, ...this.list]
					// 设置加载状态
					this.loadmore = '上拉加载更多'	
				}, 2000);
			}
		}
	}
</script>

<style>

</style>
