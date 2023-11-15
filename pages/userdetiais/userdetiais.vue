<template>
	<view class="content">
		<view class="top-bar">
			<view class="top-bar-left" @tap="goBack">
				<image class="back-img" src="../../static/image/common/back.png" mode=""></image>
			</view>
			<view class="top-bar-center">
				<view class="title">详情</view>
			</view>
			<view class="top-bar-right">
				<view class="pice"></view>
			</view>
		</view>
		<view class="main">
			<view class="column heads">
				<view class="row head">
					<view class="title">头像</view>
					<view class="user-head">
						 <image-cropper  :src="tempFilePath" @confirm="confirm" @cancel="cancel" ></image-cropper>
						 	 <image :src="cropFilePath" @tap="upload" class="user-img" ></image>
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="row" @tap="modifty('签名', dataArr.sign, false)">
					<view class="title">签名</view>
					<view class="cont">
						{{dataArr.sign}}
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="row">
					<view class="title">注册</view>
					<view class="cont">
						{{changeTime(dataArr.time)}}
					</view>
				</view>
			</view>
			
			<view class="column">
				<view class="row" @tap="modifty('昵称', dataArr.name, false)">
					<view class="title">昵称</view>
					<view class="cont">
						{{dataArr.name}}
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="row">
					<view class="title">性别</view>
					<view class="cont">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<view class="uni-input">{{array[index]}}</view>
						</picker>
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="row">
					<view class="title">生日</view>
					<view class="cont">
						<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
							<view class="uni-input">{{date}}</view>
						</picker>
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="row" @tap="modifty('电话', dataArr.tell, false)">
					<view class="title">电话</view>
					<view class="cont">
						{{dataArr.tell}}
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
				<view class="row" @tap="modifty('邮箱', dataArr.email, true)">
					<view class="title">邮箱</view>
					<view class="cont">
						{{dataArr.email}}
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
			</view>
			
			<view class="column">
				<view class="row">
					<view class="title">密码</view>
					<view class="cont">
						******
					</view>
					<view class="more">
						<image src="../../static/image/userHome/more.png" mode="aspectFit"></image>
					</view>
				</view>
			</view>
			<view class="exit">退出登录</view>
		</view>
		
		<view class="modify" :style="{bottom: -+widHeight+'px'}" :animation="animationData">
			<view class="modify-header">
				<view class="close" @tap="modifty">取消</view>
				<view class="title">签名</view>
				<view class="define" @tap="modiftyStbmit">确定</view>
			</view>
			<view class="modfiy-main">
				<input type="text" v-model="pwd" class="modfiy-pwd" placeholder="密码" placeholder-style="color:#aaa;font-weight:400;" v-show="isPaw"></input>
				<textarea v-model="data" class="modfiy-content"></textarea>
			</view>
		</view>
	</view>
</template>

<script>
	import ImageCropper from '../../uni_modules/ling-imgcropper/components/ling-imgcropper/ling-imgcropper.vue'
	import myfun from '../../commons/js/myfun.js'
	
	export default {
		data() {
			const currentDate = this.getDate({
				format: true
			})
			return {
				// 模拟数据
				dataArr: {
					name: 'GGB',
					sign: '红红火火恍恍惚惚',
					time: new Date(),
					sex: '男',
					birth: '2000-1-1',
					tell: '1243142413',
					email: '1243142413@qq.com'
				},
				// imgUrl: '../static/image/index/cat.png',
				array: ['男', '女', '未知'],              // 性别选择器
				index: 0,                                // 默认值
				date: currentDate,
				cropFilePath:'../static/image/index/cat.png',
				tempFilePath: '',
				headimg: '',
				modiftyTitle: '',
				data: '修改的内容',                       // 修改个人信息
				pwd: '',                                 // 原密码
				isPaw: false,                            // 是否显示密码
				animationData: {},                       // 动画
				isModfiy: false,                         // 动画开关
				widHeight: '',                           // 页面高度
			};
		},
		components: {ImageCropper},
		computed: {
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			}
		},
		onReady: function() {
			this.getElementStyle();
		},
		methods: {
			// 返回到登录页面
			goBack: function() {
				uni.navigateBack({
					delta: 1,
				})
			},
			
			// 性别选择器
			bindPickerChange: function(e) {
			    console.log('picker发送选择改变，携带值为', e.detail.value)
			    this.index = e.detail.value
			},
			
			// 生日选择器
			bindDateChange: function(e) {
				this.date = e.detail.value
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
	
				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			
			// 图片裁剪
			upload() {	
			    uni.chooseImage({
			        count: 1, //默认9
			        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			        sourceType: ['album', 'camera'], //从相册选择
			        success: (res) => {
			            this.tempFilePath = res.tempFilePaths[0]
										
			        }
			      });
			},
			confirm(e) {
				this.tempFilePath = ''
				this.cropFilePath = e.detail.tempFilePath
			  	this.headimg = e.detail.tempFilePath;
			},
			cancel() {
			    console.log('canceled')
			},
			uploadimg(){
				if(this.headimg!='') {
					uni.showLoading({
						title: '上传中'
					});	 
					uni.uploadFile({url:'服务器上传图片地址',
						filePath: this.headimg,
						name: 'file',
						fileType:'image',
						formData:{上传附加数据},
						success: (uploadFileRes) => { 
						
						
						},
						complete() {
							uni.hideLoading();
						},
						fail(e) {
							console.log("this is errormes "+e.message)	
						}	
					});
				} else {
					uni.showModal({
						title: '提示',
						content: '没有选择图片',
						showCancel:false
					});					
				}			  
			},
			cancel() {
				console.log('canceled')
			},
			
			// 获取页面高度
			getElementStyle: function() {
				const query = uni.createSelectorQuery().in(this);
				query.select('.modify').boundingClientRect(data => {
					this.widHeight = data.height;
					console.log("高度："+this.widHeight)
				}).exec();
			},
			
			// 修改个人信息弹窗
			modifty: function(type, data, isPaw) {
				this.modiftyTitle = type;
				this.data = data;
				this.isPaw = isPaw;
				this.isModfiy = !this.isModfiy;
				var animation = uni.createAnimation({
					duration: 300,
				    timingFunction: 'ease',
				})
				
				if(this.isModfiy) {
					animation.bottom(300).top('50%').step();
				} else {
					animation.bottom(-this.isModfiy).top(1100).step();
				}
				this.animationData = animation.export();
			},
			
			// 信息修改确认
			modiftyStbmit: function() {
				this.modifty();
			},
			
			// 时间处理
			changeTime: function(date) {
				return myfun.detialTime(date);
			},
		}
	}
</script>

<style lang="scss">
	@import "../../commons/css/common.scss";
	
	.top-bar {
		background: rgba(255, 255, 255, 0.96);
		border-bottom: 1px solid $uni-border-color;
		
		.top-bar-left {
			width: 88rpx;
			height: 88rpx;
			.back-img {
				margin-top: 21rpx;
				width: 52rpx;
				height: 52rpx;
			}
		}
		
		.top-bar-center {
			font-size: 40rpx;
		}
		
	}
	
	.main{
		padding-top: 70rpx;
		display: flex;
		flex-direction: column;
		.column {
			display: flex;
			flex-direction: column;
			padding-top: 12rpx;
			width: 100%;
			border-bottom: 1px solid $uni-border-color;
			.row {
				display: flex;
				flex-direction: row;
			}
			.title {
				flex: none;
				
				padding: 0 $uni-spacing-col-base;
				font-size: 40rpx;
				color: $uni-text-color;
				line-height: 112rpx;
			}
			.head {
				height: 148rpx;
				display: flex;
				align-items: center;
			}
			.user-head {
				flex: auto;
				
			}
			.user-img{
				margin-top: 22rpx;
				width: $uni-img-size-lg;
				height:$uni-img-size-lg;
				border-radius: $uni-border-radius-base;
			}

			.cont{
				flex: auto;
				font-size: $uni-font-size-lg;
				color:$uni-text-color-grey;
				line-height: 112rpx;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
			}
			.more{
				flex: none;
				height: 112rpx;
				display: flex;
				align-items: center;
				margin: 0 32rpx;
				image {
					width: 52rpx;
					height: 52rpx;
				}
			}
		}
		.exit {
			margin-top: 160rpx;
			text-align: center;
			font-size: $uni-font-size-lg;
			color: $uni-color-warning;
			line-height:88rpx;
		}
	}
	
	// 修改个人信息弹窗
	.modify {
		// position: absolute;
		top: 1400px;
		left: 50%;
		transform: translate(-50%, -70%);
		position: fixed;
		// left: 50%;
		// top: 50%;
		z-index: 1002;
		height:800rpx;
		width: 600rpx;
		// background-color:#eec;
		background-image:linear-gradient(-225deg,#E3FDF5 0%, #FFE6FA 100%);		
		border-radius: 18rpx;
		.modify-header {
			width:100%;
			height:88rpx;
			padding-top: var(--status-bar-height);
			display: flex;
			flex-direction: row;
			align-items: center;
			border-bottom: 1px solid $uni-border-color;
			.close {
				padding-left: 32rpx;
				font-size:$uni-font-size-lg;
				color: $uni-text-color;
				line-height: 44px;
			}
			.title{
				flex: auto;
				text-align: center;
				font-size: 40rpx;
				color:$uni-text-color;
				line-height:88rpx;
			}
			.define{
				padding-right:$uni-spacing-col-base;
				font-size: $uni-font-size-lg;
				color:$uni-color-success;
				line-height:44rpx;
			}
		}
		.modfiy-main{
			display: flex;
			padding: $uni-spacing-col-base;
			flex-direction: column;
			.modfiy-pwd {
				margin-bottom: $uni-spacing-col-base;
				padding: 0 20rpx;
				box-sizing: border-box;
				flex: auto;
				width:100%;
				height: 88rpx;
				background: #fff;
				border-radius: $uni-border-radius-base;
				font-size: $uni-font-size-lg;
				color: $uni-text-color;
				line-height:88rpx;
			}

			.modfiy-content {
				padding: 16rpx 20rpx;
				box-sizing: border-box;
				flex: auto;
				width: 100%;
				height:186rpx;
				background: #fff;
				border-radius: $uni-border-radius-base;
				font-size: $uni-font-size-lg;
				color:$uni-text-color;
				line-height: 44rpx;
			}
		}
	}



</style>
