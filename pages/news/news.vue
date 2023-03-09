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
	</view>
</template>

<script>
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	export default {
		components: {
			uniNavBar
		},
		data() {
			return {
				tabIndex: 0,
				tabBars: [{name: '关注'}, {name: '话题'}]
			}
		},
		methods: {
			openAddInput() {
				console.log('add input')
				uni.navigateTo({
					url: '/pages/add-input/add-input'
				})
			},
			changeTab(index) {
				this.tabIndex = index
			}
		}
	}
</script>

<style>

</style>
