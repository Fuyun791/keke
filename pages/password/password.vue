<template>
	<view class="page">
		<view class="card">
		<view class="input-group">
			<view class="input-row border">
				<text class="title">原密码：</text>
				<view style="padding-top: 20rpx;">
					<input type="password" placeholder="请输入原密码"></input>
				</view>
			</view>
			<view class="input-row border">
				<text class="title">新密码：</text>
				<view style="padding-top: 20rpx;">
					<input type="password" placeholder="请输入新密码"></input>
				</view>
			</view>
			<view class="input-row">
				<text class="title">确认密码：</text>
				<view style="padding-top: 20rpx ;">
					<input type="password" placeholder="请输入确认密码"></input>
				</view>	
			</view>
		</view>
		<view class="btn-row">
			<button class="btn" form-type="submit" type="primary">提交</button>
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
		background-color: #007AFF;
	}
</style>
