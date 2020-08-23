<template>
	<view class="page">
		<view class="card">
			<form @submit="formSubmit" @reset="formReset">
		<view class="input-group">
			<view class="input-row border" >
				<text class="title">学号：</text>
				<view style="padding-top: 20rpx;">
					<input name="username" placeholder=" 请输入学号"></input>
				</view>
			</view>
			<view class="input-row border">
				<text class="title">新密码：</text>
				<view style="padding-top: 20rpx;">
					<input name="password" type="password" password="ture" placeholder=" 默认密码为Hziee+学号" ></input>
				</view>
			</view>
		</view>
		<view class="btn-row">
			<button class="btn" form-type="submit" type="primary">提交</button>
		</view>
		</form>
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
			formSubmit: function(e) {
				let formdata = e.detail.value
				uni.request({
					url: getApp().globalData.requestUrl+ 'login',
					method: 'POST',
					data: {
						username: formdata.username,
						password: formdata.password
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						try {
							if(res.data.status != 200) {
								uni.showToast({
									title: "账号或密码错误",
									duration: 2000
								})
								return ;
							}
							let result = res.data.result
							let array = result.split("_")
							//设置缓存
							uni.setStorageSync("studentNum", formdata.username)
							uni.setStorageSync("collegeId", array[0])
							uni.setStorageSync("tt", array[1])
							//请求学生信息
							uni.request({
								url: getApp().globalData.requestUrl + 'student-info/list-studentInfo',
								data: {
									studentNum: formdata.username
								},
								header: {
									"Authorization": "Bearer WX_" + array[1]
								},
								success: res => {
									let studentInfo = res.data.result
									if(studentInfo != null) {
										uni.setStorageSync("studentName",studentInfo.adminName)
										uni.setStorageSync("studentAvatarUrl",studentInfo.adminPic)
										let pages= getCurrentPages()
										let prevPage = pages[pages.length -2]
										console.log(prevPage)
										if(prevPage.route == 'pages/my/my') {
											console.log('my')
											prevPage.setData({
												imageSrc: studentInfo.adminPic,
												name: studentInfo.adminName
											})
										}
										uni.navigateBack({
											delta: 1
										})
									}
								}
							})
						} catch (e) {
							console.log(e)
						}
					},
					fail: (res) => {
						uni.showToast({
							title: "账号或密码错误",
							duration: 2000
						})
					}
				})
			},
			formReset: function(e) {
				uni.navigateBack({
					delta: 1
				})
			}
		},
		onLoad: function() {
		}
	}
</script>

<style>
	.page {
			height: 600upx;
			padding: 25upx;
			background: linear-gradient(#FF978D, #FFBB69);
	}
	
	.card{
		background: #fff;
		border-radius: 12upx;
		padding: 10upx 25upx;
		box-shadow: 0 6upx 18upx rgba(0,0,0,0.12);
		position: relative;
		margin-top: 300upx;
	}
	
	.content {
		display: flex;
		flex: 1;
		flex-direction: column;
		width: 100%;
		padding: 10px;
	}
	
	.input-group {
		position: relative;
		width: 750rpx;
		margin-top: 40px;
	}
	
	.input-group::before {
		position: absolute;
		right: 0;
		top: 0;
		left: 0;
		height: 1px;
		content: '';
		-webkit-transform: scaleY(.5);
		transform: scaleY(.5);
	}
	
	.input-group::after {
		position: absolute;
		right: 0;
		bottom: 0;
		left: 0;
		height: 1px;
		content: '';
		-webkit-transform: scaleY(.5);
		transform: scaleY(.5);
	}
	
	.input-row {
		display: flex;
		flex-direction: row;
		position: relative;
		font-size: 16px;
		line-height: 40px;
	}
	
	.input-row .title {
		width: 90px;
		padding-left: 15px;
	}
	
	.input-row.border::after {
		position: absolute;
		right: 0;
		bottom: 0;
		left: 8px;
		height: 1px;
		content: '';
		-webkit-transform: scaleY(.5);
		transform: scaleY(.5);
		background-color: #c8c7cc;
	}
	
	.btn-row {
		margin-top: 25px;
		padding: 10px;
	}
	
	.btn{
		color: #F7F7F7;
	}
</style>
