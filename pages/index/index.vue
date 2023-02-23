<template>
	<view >
		<view class="item" @click="handleMap()">
			<text>百度地图</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		onLoad() {

		},
		methods: {
            handleMap(){
				uni.navigateTo({
				       url: '/pages/map/map'
				     });
			}
		}
	}
</script>

<style>
	.item{
		width:200rpx;
		height:200rpx;
		background-color: #ececec;
		text-align: center;
		line-height: 200rpx;
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
