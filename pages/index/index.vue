<template>
	<view class="content">
		<view class="top-bar">
			<navigator url="../userHome/userHome?id=aaa" class="top-bar-left" hover-class="none">
				<image src="../../static/image/index/pxx.png" mode=""></image>
			</navigator>
			<view class="top-bar-center">
				yike
			</view>
			<view class="top-bar-right">
				<image class="search" @tap="toSearch" src="../../static/image/index/search.png" mode=""></image>
				<image class="add" src="../../static/image/index/add.png" mode="" @tap="toBuildGroup"></image>
			</view>
		</view>
		
		<view class="main">
			<view class="friends">
				<view class="friend-list">
					<view class="friend-list-l">
						<text class="tip">1</text>
						<view class="newFriends">
							<image class="friends" src="../../static/image/index/friendAdd.png" mode=""></image>
						</view>
					</view>
					<view class="friend-list-r">
						<view class="top">
							<view class="name">好友申请</view>
							<view class="time">12:00</view>
						</view>
						<view class="message">
							我是傻逼
						</view>
					</view>
				</view>
			</view>
			<view class="friends">
				<navigator url="../chatRoom/chatRoom" class="friend-list" v-for="(item,index) in friends" :key="item.id">

					<view class="friend-list-l">
						<text class="tip" v-show="item.tip>0">{{item.tip}}</text>
						<image :src="item.imgUrl" mode=""></image>
					</view>
					<view class="friend-list-r">
						<view class="top">
							<view class="name">{{item.name}}</view>
							<view class="time">{{changeTime(item.time)}}</view>
						</view>
						<view class="message">
							{{item.message}}
						</view>
					</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	import datas from '../../commons/js/datas.js'
	import myfun from '../../commons/js/myfun.js'
	
	export default {
		data() {
			return {
				friends: [],
			}
		},
		onLoad() {
			this.getFriends();
		},
		methods: {
			// 获取时间
			changeTime: function(date) {
				return myfun.dateTime(date);
			},
			
			getFriends: function() {
				this.friends = datas.friends();
				for (let i = 0; i < this.friends.length; i++) {
					this.friends[i].imgUrl = '../../static/image/index/' + this.friends[i].imgUrl;
				}
				// console.log(this.friends);
			},
			
			// 跳转到登录页面
			toSearch: function() {
				uni.switchTab({
					url: '../search/search',
				})
			},
			
			toBuildGroup:function(){
				uni.navigateTo({
					url: '../buildgroup/buildgroup',
				})
			},
			
			// 跳转到对应的聊天页面
			navigateTo:function(index){
				uni.navigateTo({
					url:'../chatRoom/chatRoom'
				})
			}
		}
	}
</script>

<style lang="scss">
	@import "../../commons/css/common.scss";
	.content {
		display: flex;
		flex-direction: column;
		padding-top: var(--status-bar-height);
		padding-bottom: $uni-spacing-col-base;
		
		.top-bar {
			border-bottom: 1px solid $uni-border-color;
			.top-bar-right {
				image {
					width: 48rpx;
					height: 48rpx;
				}
			}
			
		}
	}
	
	.main {
		padding-top: 104rpx;
	}
	
	.friend-list {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 96rpx;
		padding: 16rpx $uni-spacing-col-base;
		&:active {
			background-color: $uni-bg-color-grey;
		}
		
		.friend-list-l {
			position: relative;
			width: 96rpx;
			height: 96rpx;
			image {
				width: 96rpx;
				height: 96rpx;
				border-radius: $uni-border-radius-base;
				background-color: $uni-color-primary;
			}
			.tip {
				position: absolute;
				top: -8rpx;
				left: 68rpx;
				min-width: 24rpx;
				height: 36rpx;
				padding: 0 6rpx;
				z-index: 100;
				background: $uni-color-warning;
				border-radius: 18rpx;
				font-size: $uni-font-size-sm;
				color: $uni-text-color-inverse;
				line-height: 36rpx;
				text-align: center;
			}
			.newFriends {
				border-radius: $uni-border-radius-base;
				background-color: $uni-color-primary;
				padding: 22rpx;
				.friends {
					width: 52rpx;
					height: 52rpx;
				}
			}
		}
		
		.friend-list-r {
			padding-left: 32rpx;
			display: flex;
			flex-direction: column;
			width:calc(100% - 60rpx);
			.top {
				display: flex;
				justify-content: space-between;
				align-items: center;
				width:100%;
				.name {
					font-size: 36rpx;
					font-weight: 400;
					color: $uni-text-color;
					line-height: 50rpx;
				}
				.time {
					font-size: $uni-font-size-sm;
					color: $uni-text-color-disable;
					line-height: 50rpx;
				}
			}
			
			.message {
				font-size: $uni-font-size-base;
				color: $uni-text-color-grey;
				line-height: 40rpx;
				display: -webkit-box;
				-webkit-box-orient: vertical;
				-webkit-line-clamp: 1;
				overflow: hidden;
			}
		}
	}
		
	
</style>
