<template>
	<view>
		<view>
			{{total}}
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				unit: 1.5,
				count: 3
			}
		},
		computed: {
			total() {
				return this.unit * this.count
			}
		},
		watch: {	
		},
		methods: {
		}
	}
</script>

<style>
	
</style>
