<template>
	<view>
		<!-- 自定义导航 -->
		<uni-nav-bar left-icon="back" statusBar :border="false" @clickLeft="back">
			<view class="flex align-center justify-center w-100">
				所有人可见<text class="iconfont icon-shezhi"></text>
			</view>
		</uni-nav-bar>
		<!-- 文本域 -->
		<textarea v-model="content" placeholder="说一句话吧" class="uni-textarea px-2" />
		<!-- 多图上传 -->
		<upload-image :show="show" ref="uploadImage" :list="imageList" @change="changeImage"></upload-image>
		<!-- 底部操作条 -->
		<view class="fixed-bottom bg-white flex align-center" style="height: 85rpx;">
			<view class="iconfont icon-caidan footer-btn animated" hover-class="jello"></view>
			<view class="iconfont icon-huati footer-btn animated" hover-class="jello"></view>
			<view class="iconfont icon-tupian footer-btn animated" hover-class="jello" @click="iconClickEvent('uploadImage')"></view>
			
			<view class="bg-main text-white ml-auto flex justify-center align-center rounded mr-2 animated" hover-class="jello" style="width: 140rpx;height: 60rpx;">发送</view>
		</view>
	</view>
</template>

<script>
	import UniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	import UploadImage from '@/components/common/upload-image.vue';
	export default {
		components: {
			UniNavBar,
			UploadImage
		},
		data() {
			return {
				content: "",
				imageList: [],
				// 是否已经弹出提示框
				showBack: false
			}
		},
		computed: {
			show() {
				return this.imageList.length > 0
			}
		},
		// 监听返回
		// onBackPress() { },
		// 页面加载时
		onLoad() {
			uni.getStorage({
				key: 'add-input',
				success: (res) => {
					if(res.data) {
						let result = JSON.parse(res.data)
						this.content = result.content;
						this.imageList = result.imageList;
					}
				}
			})
		},
		methods: {
			iconClickEvent(e) {
				switch(e) {
					case 'uploadImage':
						this.$refs.uploadImage.chooseImage();
						break;
				}
			},
			changeImage(e) {
				this.imageList = e
			},
			store() {
				uni.setStorage({
					key: 'add-input',
					data: JSON.stringify({
						content: this.content,
						imageList: this.imageList
					})
				})
			},
			back() {
				if ((this.content !== '' || this.imageList.length > 0) && !this.showBack ) {
					uni.showModal({
						content: '是否要保存为草稿？',
						showCancel: true,
						cancelText: '不保存',
						confirmText: '保存',
						success: res => {
							// 点击确认
							if (res.confirm) {
								this.store()
							} else { // 点击取消，清除缓存
								uni.removeStorage({
									key:"add-input"
								})
							}
							// 手动执行返回
							uni.navigateBack({ delta: 1 });
						},
					});
					this.showBack = true
					return true
				}
			}
		}
	}
</script>

<style>
	.footer-btn {
		width: 86rpx;
		height: 86rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 50rpx;
	}

</style>
