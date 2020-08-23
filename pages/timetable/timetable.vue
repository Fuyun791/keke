<template>
	<view>
		<!-- 日期表 -->
		 <view class="layout1" style="height: 200upx; background:linear-gradient(#ffa86a,#ffaa00);">
			<view class="view1">{{month}}月</view>
			<view style="padding-top: 40upx;">
				<view style="color: #FFFFFF; font-size: 40upx;">
					<text class="right1">日</text>
					<text class="right1">一</text>
					<text class="right1">二</text>
					<text class="right1">三</text>
					<text class="right1">四</text>
					<text class="right1">五</text>
					<text class="right1">六</text>
				</view>
				<view class="layout1">
					<view v-bind:class="sum==day?'view3' : 'view2'">{{sum}}</view>
					<view v-bind:class="mon==day?'view3' : 'view2'">{{mon}}</view>
					<view v-bind:class="tue==day?'view3' : 'view2'">{{tue}}</view>
					<view v-bind:class="wed==day?'view3' : 'view2'">{{wed}}</view>
					<view v-bind:class="thu==day?'view3' : 'view2'">{{thu}}</view>
					<view v-bind:class="fri==day?'view3' : 'view2'">{{fri}}</view>
					<view v-bind:class="sat==day?'view3' : 'view2'">{{sat}}</view>
				</view>
			</view>
		</view>
		
		 <!-- 状态栏 -->
		 <view class="btn-row" style="padding-top: 20upx;padding-bottom: 20upx;">
		 	<view :class="{bottom:isActive,bottom1:!isActive}" v-on:click="show(1)">待完成</view>
		 	<view :class="{bottom:isActive2,bottom1:!isActive2}" v-on:click="show(2)">未完成</view>
		 	<view :class="{bottom:isActive3,bottom1:!isActive3}" v-on:click="show(3)">已完成</view>
		 </view>
		 
		<!-- 待完成 -->
		<view v-if="showNum == 1" style="margin-top: 10upx;">
			<!-- 计划 -->
			<view class="msg-card">
				<view class="zt-box">
					<view style="font-size: 32rpx; margin:0 auto;">小目标</view>
				</view>
				<view style="padding: 20rpx;">
						<view >今日目标好好学习</view>
						<view class="btn-row" style="margin-top: 50rpx;">
						<view class="box-time">8:00~12:00</view>
						<view class="box-btn">打卡</view>
						</view>
				</view>
			</view>
			<!-- 课程 -->
			<view class="msg-card">
				<view class="zt-box">
					<view style="font-size: 32rpx; margin:0 auto;">学校课程</view>
				</view>
				<view style="padding: 20rpx;">
						<view >计算机原理</view>
						<view style="right: 0; font-size: 24rpx; color: #777777;">唐老师</view>
						<view style="right: 0; font-size: 24rpx; color: #777777;">E201</view>
						<view class="btn-row">
						<view class="box-time">8:00~12:00</view>
						<view class="box-btn">打卡</view>
						</view>
				</view>
			</view>
			
			<view class="add-icon"> +</view>
		</view>
		
		<!-- 未完成 -->
		<view v-if="showNum == 2" style="margin-top: 10rpx;"> 
		<view class="msg-card">
			<view class="zt-box">
				<view style="font-size: 32rpx; margin:0 auto;">小目标</view>
			</view>
			<view style="padding: 20rpx;">
					<view >今日目标好好学习</view>
					<view class="btn-row" style="margin-top: 50rpx;">
					<view class="box-time">8:00~12:00</view>
					<view class="box-btn" style="background-color: #C0C0C0;">未打卡</view>
					</view>
			</view>
		</view>
		</view>
		
		<!-- 已完成 -->
		<view v-if="showNum == 3" style="margin-top: 10rpx;">
		<view class="msg-card">
			<view class="zt-box">
				<view style="font-size: 32rpx; margin:0 auto;">小目标</view>
			</view>
			<view style="padding: 20rpx;">
					<view >背300个英语单词</view>
					<view class="btn-row" style="margin-top: 50rpx;">
					<view class="box-time">8:00~12:00</view>
					<view class="box-btn" style="background-color: #C0C0C0;">已打卡</view>
					</view>
			</view>
		</view>
		</view>
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				month: "", 
				day: "",
				weekDay: "",
				startDay: "",
				sum:"",
				mon:"",
				tue:"",
				wed:"",
				thu:"",
				fri:"",
				sat:"",
				
				showNum : 1,
				isActive : true,
				isActive2 : false,
				isActive3 : false,
			}
		},
		onLoad: function() {
			var date = new Date();
			this.month = date.getMonth() + 1;
			this.day = date.getDate();
			this.weekDay = date.getDay();
			
			var startDay = this.day - this.weekDay;
			date.setDate(startDay);
			this.sum = date.getDate();
			this.mon = new Date(new Date().setDate(startDay + 1)).getDate();
			this.tue = new Date(new Date().setDate(startDay + 2)).getDate();
			this.wed = new Date(new Date().setDate(startDay + 3)).getDate();
			this.thu = new Date(new Date().setDate(startDay + 4)).getDate();
			this.fri = new Date(new Date().setDate(startDay + 5)).getDate();
			this.sat = new Date(new Date().setDate(startDay + 6)).getDate();
			
			// 测试长度
			const query = uni.createSelectorQuery().in(this);
			query.select('#list').boundingClientRect(data => {
			  console.log("节点宽度为" + data.width);
			}).exec();
	    },
		methods: {
			show: function(num){
				this.showNum = num;
					if(num == 1){
						this.isActive =  true;
						this.isActive2 = false;
						this.isActive3 = false;
					}
					if(num == 2){
					this.isActive2 = true;
					this.isActive = false;
					this.isActive3 = false;
					}
					if(num == 3){
						this.isActive3 = true;
						this.isActive = false;
						this.isActive2 = false;
					}
			}
		},
	}
</script>

<style>
	view{
			font-size: 35upx;
		}
	.layout1{
		display: flex;
		flex-direction: row;
	}
	.view1{
		display: flex;
		justify-content: center;
		align-items: center;
		width: 150upx;
		font-size: 45upx;
		color: #FFFFFF;
	}
	.right1{
		margin-right: 37upx;
	}
	.view2{
		display: flex;
		justify-content: center;
		width: 50upx;
		margin-right: 25upx;
		margin-top: 20upx;
		font-size: 40upx;
		color: #FFFFFF;
	}

	.view3{
		display: flex;
		justify-content: center;
		width: 50upx;
		margin-right : 25upx;
		margin-top: 20upx;
		font-size: 40upx;
		color: #55aaff;
		background-color: #FFFFFF;
		border-radius: 50%;
	}
	
	.btn-row{
		display: flex;
		flex-direction: row;
	}
	
	.bottom1{
		width: 250rpx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		background-color: #FFFFFF;
		border-bottom: solid 8rpx #E5E5E5;
		text-align: center;
	}
	
	.bottom{
		width: 250upx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		background-color: #FFFFFF;
		color: #ffaa00;
		border-bottom: solid 8rpx #ffaa00;
		text-align: center;
	}
	
	.msg-card{
		margin: 30rpx;
		background: #FFFFFF;
		box-shadow: 6rpx 6upx 18upx #808080;
		position: relative;
		border-radius: 20rpx;
	}
	
	.zt-box{
		display: flex;
		flex-direction: row;
		background: #ffffff;
		padding: 10upx 25upx;
		border-radius: 0rpx,0rpx,20rpx,20rpx;
	}
	
	.zt-radius{
		background-color: #DD524D;
		border-radius: 50%; 
		height: 30rpx;
		width: 30rpx;
		margin-top: 10rpx;
	}
	
	.box-btn{
		height: 50rpx;
		width: 150rpx;
		font-size: 16px;
		margin-left: 400rpx;
		text-align: center;
		line-height: 50rpx;
		margin-right: 0rpx;
		color: #FFFFFF;
		background-color: #ffaa00;
		border-radius: 10rpx;
	}
	
	.box-time{
		margin-top: 10rpx;
		font-size: 28rpx;
		color:#777777 ;
	}
	
	.add-icon{
		position: fixed;
		right: 20rpx;
		bottom: 30rpx;
	}
</style>