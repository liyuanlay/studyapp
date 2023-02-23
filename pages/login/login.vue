<template>
	<view class="content" >
		<form @submit="formSubmit" style="width:80%;">
		<view>
			 <input  class="input"  type="text" placeholder="请输入用户名" />
		</view>			
		<view>
			<input   class="input"  password type="text"  placeholder="请输入密码" />			
		</view>
		<view >
			<button form-type="submit" style="border-radius: 100rpx;   background-color: #1890ff;color:#fff;" >登录</button>
		</view>		
		</form>		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {
	
		},
		methods: {
	      formSubmit(){
			   uni.navigateTo({
			          url: '/pages/index/index'
			        });
		  }
		}
	}
</script>

<style>
	.content{
		width: 100%;
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.input{
		margin:40rpx 0;
		height:80rpx;
		border-radius: 10rpx;
		border:1px solid #c2c0c0;
	}
</style>