<template>
	<view>
		<scroll-view style="height: 100rpx;" scroll-x :scroll-into-view="scrollInto" scroll-with-animation class="scroll-row border-bottom border-light-secondary">
			<view v-for="(item, index) in tabBars" class="scroll-row-item px-3 py-2 font-md" :key="index" :id="'tab' + index" :class="tabIndex === index ? 'text-main font-lg font-weight-bold' : ''" @click="changeTab(index)">{{item.name}}</view>
		</scroll-view>
		
		<swiper :style="'height:' + scrollH + 'px'" :duration="150" :current="tabIndex" @change="onChangeTab">
			<swiper-item v-for="(item, index) in newsList" :key="index">
				<scroll-view scroll-y :style="'height:' + scrollH + 'px'" @scrolltolower="loadmore(index)">
					<template v-if="item.list.length > 0">
						<!-- 列表 -->
						<block v-for="(item2, index2) in item.list" :key="index2">
							<!-- 列表样式 -->
							<common-list :item="item2" :index="index2" @follow="follow" @doSupport="doSupport"></common-list>
							<!-- 全局分割线 -->
							<divider></divider>
						</block>
						<!-- 上拉加载 -->
						<load-more :loadmore="item.loadmore"></load-more>
					</template>
					<template v-else>
						<no-thing />
					</template>
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
	import CommonList from '@/components/common/common-list.vue';
	import LoadMore from '@/components/common/load-more.vue';
	export default {
		components: {
			CommonList,
			LoadMore
		},
		data() {
			return {
				// 列表高度
				scrollH: 600,
				// 顶部选项卡
				scrollInto: "",
				tabIndex: 0,
				tabBars: [{name: '关注'},{name: '推荐'},{name: '体育'},{name: '热点'},{name: '财经'},{name: '娱乐'},{name: '军事'},{name: '历史'},{name: '本地'}],
				newsList: []
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success: res => {
					this.scrollH = res.windowHeight - uni.upx2px(101)
				}
			})
			 // 根据选项生成列表
			 this.getData()
		},
		// 监听点击导航栏搜索框
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url:"/pages/search/search"
			})
		},
		// 监听导航按钮点击事件
		onNavigationBarButtonTap() {
			uni.navigateTo({
				url:"/pages/add-input/add-input"
			})
		},
		methods: {
			//获取数据
			getData() {
				var arr = []
				for(let i = 0; i < this.tabBars.length; i++) {
					let obj = {
						loadmore: "上拉加载更多",
						list: []
					}
					if(i < 2) {
						obj.list = demo
					}
					arr.push(obj)
				}
				this.newsList = arr;
			},
			// 监听滑动
			onChangeTab(e) {
				this.changeTab(e.detail.current)
			},
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
			},
			loadmore(index) {
				let item = this.newsList[index];
				if(item.loadmore !== '上拉加载更多') return;
				item.loadmore = '加载中...';
				setTimeout(() => {
					item.list = [...item.list, ...item.list]
					item.loadmore = '上拉加载更多'
				}, 2000)
			}
		}
	}
</script>

<style>
	
</style>
