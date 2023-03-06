<template>
	<!-- 列表样式 -->
	<view class="p-2">
		<!-- 头像昵称 ｜ 关注按钮 -->
		<view class="flex align-center justify-between">
			<view class="flex align-center">
				<image :src="item.userPic" class="rounded-circle mr-2" style="width: 65rpx;height: 65rpx;" lazy-load @click="openSpace"></image>
				<view>
					<view class="font" style="line-height: 1.5;">{{item.username}}</view>
					<text class="font-md text-light-muted" style="line-height: 1.5;">{{item.newstime}}</text>
				</view>
			</view>
			<view @click="follow" v-if="!item.isFollow" class="flex align-center justify-center rounded bg-main text-white animated faster" hover-class="jello" style="width: 90rpx;height: 50rpx;">
				关注
			</view>
		</view>
		<!-- 标题 -->
		<view class="font my-1" @click="openDetail">{{item.title}}</view>
		<!-- 图片 -->
		<image v-if="item.titlePic" :src="item.titlePic" class="rounded w-100" style="height: 350rpx;"></image>
		<!-- 图标按钮 -->
		<view class="flex align-center">
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="jello text-main" :class="item.support.type === 'support' ? 'support-active' : ''">
				<text class="iconfont icon-dianzan2 mr-2" @click="doSupport('support')"></text>
				<text>{{item.support.support_count || '支持'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="jello text-main" :class="item.support.type === 'unsupport' ? 'support-active' : ''">
				<text class="iconfont icon-cai mr-2"  @click="doSupport('unsupport')"></text>
				<text>{{item.support.unsupport_count || '反对'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="jello text-main">
				<text class="iconfont icon-pinglun2 mr-2" @click="openDetail"></text>
				<text>{{item.comment_count || '评论'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1 animated faster" hover-class="jello text-main">
				<text class="iconfont icon-fenxiang mr-2" @click="openDetail"></text>
				<text>{{item.share_num || '分享'}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			item: Object,
			index: Number
		},
		methods: {
			// 打开个人空间
			openSpace() {
				console.log('打开个人空间')
			},
			// 关注
			follow() {
				this.$emit('follow', this.index)
			},
			// 进入详情页
			openDetail() {
				console.log('进入详情页')
			},
			// 顶踩操作
			doSupport(type) {
				this.$emit('doSupport', {
					type,
					index: this.index
				})
			}
		}
	}
</script>

<style>
	.support-active {
		color: #FF4A6A;
	}
</style>