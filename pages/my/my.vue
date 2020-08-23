<template>
	<view>
		<!-- 头部 -->
		<view class="layout1" style="padding: 10px 20upx;display: flex;align-items: center;justify-content: space-between;">
			<view style="display: flex;align-items: center;">
				<image style="width: 65px;height: 65px;border-radius: 50%;" :src="imageSrc"></image>
				<view>
					<view style="font-size: 13pt;font-weight: bold;margin-left: 8px;" v-if="name == '登录'" @click="getUserInfo">{{name}}</view>
					<view style="font-size: 13pt;font-weight: bold;margin-left: 8px;" v-else>{{name}}</view>
				</view>
			</view>
			<view>
				<uni-icons type="arrowright" size="17" color="#dadada"></uni-icons>
			</view>
<!-- 			<view>
				<image style="width: 38upx;height: 38upx;" src="../../static/icon/set.png"></image>
			</view> -->
		</view>
		<view style="height: 7px;width: 100%;background-color: #F7F7F7;"></view>
		<!-- 我的数据模块 -->
		<view style="font-size: 14pt;font-weight: bold;padding: 20upx;">我的数据</view>
		<view class="content" style="background-color: #F7F7F7;">
			<scroll-view scroll-x="true" class="scroll">
				<view class="box" v-for="(item,index) in listarr1" :key="index">
					<view style="text-align: center;">
						<image style="margin-right: 10upx;width: 30upx;height: 30upx;" :src="item.url"></image>
						{{item.title}}
					</view>
					<view style="text-align: center;">{{item.completion}}</view>
				</view>
			</scroll-view>
		</view>
		<!-- 本周排名模块 -->
		<uni-card isShadow="true">
				<view style="color: #808080;">本周排名<image style="margin-left: 470upx; width: 30upx;height: 30upx;"></image></view>
				<view class="layout1" style="margin-top: 10upx;">
					<view>学校</view>
					<view style="margin-left: 460upx;">{{shcool}}</view>
				</view>
				<view class="layout1" style="margin-top: 10upx;">
					<view>全国</view>
					<view style="margin-left: 460upx;">{{country}}</view>
				</view>
		</uni-card>
		<!-- 校园功能模块 -->
		<view style="font-size: 14pt;font-weight: bold;padding: 20upx;">校园功能</view>
		<!-- 第一行的三个功能 -->
		<view class="layout2" style="text-align: center;margin-top: 10upx;">
			<view @click="skip1">
				<view><image class="image1" src="../../static/icon/evaluate.png"></image></view>
				<view>教学考评</view>
			</view>
			<view @click="skip2">
				<view><image class="image1" src="../../static/icon/room.png"></image></view>
				<view>空教室</view>
			</view>
			<view @click="skip3">
				<view><image class="image1" src="../../static/icon/result.png"></image></view>
				<view>我的成绩</view>
			</view>
		</view>
		<!-- 第二行的三个功能 -->
		<view class="layout2" style="margin-top: 60upx;margin-bottom: 40upx; text-align: center;">
			<view @click="skip4">
				<view><image class="image1" src="../../static/icon/test.png"></image></view>
				<view>我的考试</view>
			</view>
			<view @click="skip5">
				<view><image class="image1" src="../../static/icon/course.png"></image></view>
				<view>我的课程</view>
			</view>
			<view @click="skip6">
				<view><image class="image1" src="../../static/icon/collect.png"></image></view>
				<view>我的收藏</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showAvatar: true,
				imageSrc: 'http://123.57.57.136/jklimage/cat.png',
				name: "登录",
				words: "好好读大学",
				shcool: "28",
				country: "2200",
				listarr1:[
					{title:"今日学习目标",completion:"1/5",url:"../../static/icon/dot1.png"},
					{title:"本周目标",completion:"0/5",url:"../../static/icon/dot2.png"},
					{title:"连续打卡",completion:"28",url:"../../static/icon/dot3.png"},
					{title:"今日学习目标",completion:"1/5",url:"../../static/icon/dot4.png"},
					{title:"本周目标",completion:"0/5",url:"../../static/icon/dot1.png"},
				],
			}
		},
		computed: {

		},
		methods: {
			getUserInfo() {
				uni.navigateTo({
					url: '../index/login'
				})
			},
			skip1:function(){
				uni.navigateTo(
					{url: 'evaluation'}
				);
			},
			skip2:function(){
				uni.navigateTo(
					{url : 'emptyroom'}
				);
			},
			skip3:function(){
				uni.navigateTo(
					{url : 'mark'}
				);
			},
			skip4:function(){
				uni.navigateTo(
					{url: 'myExam'}
				);
			},
			skip5:function(){
				uni.navigateTo(
					{url : 'myCourse'}
				);
			},
			skip6:function(){
				uni.navigateTo(
					{url : 'myCollect'}
				);
			}
		},
		onShow: function() {
			try {
				if(uni.getStorageSync("studentName") != '') {
					this.name = uni.getStorageSync("studentName")
					this.imageSrc = uni.getStorageSync("studentAvatarUrl")
				}
			} catch (e) {
				
			}
		}
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
	.layout2{
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}
	.layout3{
		display: flex;
		flex-direction: column;
		justify-content: space-around;
	}
	.left1{
		margin-left: 30upx;
	}
	.top1{
		margin-top: 30upx;
	}
	/* 横向滑动盒子 */
	.content {
        padding:20upx;
		padding-left: 0upx;
    }
    .scroll {
        width: 100%;
        overflow: hidden;
        white-space: nowrap;
    }
 
    .box {
        display: inline-block;
		padding: 10upx;
        width: 300upx;
        height: 100upx;
        background: #FFFFFF;
        margin-left: 30upx;
    }
	.image1{
		width: 80upx;
		height: 80upx;
	}
</style>
