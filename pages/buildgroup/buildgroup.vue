<template>
	<view class="content">
		<view class="top-bar">
			<view class="top-bar-left" @tap="goBack">
				取消
			</view>
			<view class="top-bar-center">
				<view class="title">创建群聊</view>
			</view>
			<view class="top-bar-right">
				<view class="pice"></view>
			</view>
		</view>
		
		<view class="main">
			<!-- 群头像 -->
			<view class="image">
				<image src="../../static/image/index/groupChat.jpg"></image>
			</view>
			
			<!-- 群名称 -->
			<view class="group-name">
				<input type="text" class="group-name-input" placeholder="为群聊取个名字吧"/>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
 		methods: {
			goBack:function(){
				uni.switchTab({
					url:'/pages/index/index'
				})
			},
			
		}
	}
</script>

<style lang="scss">
	@import '../../commons/css/common.scss';
	.top-bar{
		background:rgba(255,255,255,0.96);
		border-bottom:1px solid $uni-border-color;
	}
	.main{
		display:flex;
		flex-direction: column;
		
		align-items: center;
		
		image{
			width:350rpx;
			height:350rpx;
			margin-top:50rpx;
			border-radius: 30rpx;
			box-shadow:0px 36rpx 40rpx 0px rgba(39,40,50,0.1);
		}
		.group-name{
			width:650rpx;
			height:100rpx;
			border-radius: 40rpx;
			text-align: center;
			margin-top:100rpx;
			background-color: #f0f2f4;
			// font-size:40rpx;
			line-height:100rpx;
			margin-left:70rpx;
			margin-right:70rpx;
			.group-name-input{
				height:100%;
			}
		}
		
	}
</style>
