<template>
	<view>
		<!-- 小组头像和背景 -->
		<view class="view1" style="height: 300upx; background-image: url(http://123.57.57.136/jklimage/hu3.jpg);">
			<image :src="url1" style="margin-top: 100upx; border: 5upx solid #FFFFFF; width: 140upx;height: 140upx;border-radius: 50%;"></image>
		</view>
		<view class="layout1" style="padding: 20upx;">
			<view v-if="showNum == 1" class="view2" style="margin-left: 500upx;" @click="guanzhu()">关注</view>
			<view v-if="showNum == 2" class="view3" style="margin-left: 500upx;">已关注</view>
		</view>
		<!-- 展示该小组的所有的文章 -->
		<uni-card isShadow="true" v-for="(item,index) in topicInfoList" :key="index">
			<view class="layout1" @click="skip(item.id)">
				<view style="width:430upx;">
					<view class="font1">{{item.topicName}}</view>
					<view class="font2">{{item.topicBrief}}</view>
				</view>
				<view style="margin-left: 5upx;">
					<image style="width: 200upx;height: 130upx;border-radius: 40upx;" :src="item.topicImg"></image>
					<view class="layout1">
						<view class="font2">10回复</view>
						<view class="font2">5分钟前</view>
					</view>
				</view>
			</view>
		</uni-card>
		<uni-popup ref="guanzhu" type="center">
			<view class="view4">关注成功~</view>
		</uni-popup>
	</view>
</template>

<script>	
	import currentDate from "../../static/js/currentDate.js"
	
	export default {
		data() {
			return {
				url1:"",
				groupId:"",
				topicInfoList:[],
				timer:"",
				//用来测试是否关注过
				test1:[],
				showNum:2
			}
		},
		onLoad:function(option){
			uni.setNavigationBarTitle({
				title:option.title
			})
			this.url1 = option.url1
			this.groupId = option.groupId
			this.getTheGroupList2()
			this.getTopicInfoList()
		},
		methods: {
			//点击关注
			guanzhu(){
				this.test1 = []
				//获取当前时间
				this.timer = currentDate.getDate()
				//执行关注操作
				this.insertMyGroup()
			},
			//跳转到话题页面
			skip(id){
				uni.navigateTo(
					{url:'article?id=' + id}
				);
			},
			//查询是否关注
			getTheGroupList2:function(){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'stu-group-relation/list-theGroup',
							data: {
								studentNum: studentNum,
								groupId:this.groupId,
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.test1 = res.data.result
								if(this.test1.length == 0){
									this.showNum = 1
								}else{
									this.showNum = 2
								}
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.test1 = null
					}
				}catch (e) {
					console.log(e)
				}
			},
			//执行关注操作
			insertMyGroup:function(){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'stu-group-relation/insertMyGroup',
							data: {
								studentNum: studentNum,
								groupId:this.groupId,
								joinTime:this.timer,
								dataCreate:this.timer,
								dataModified:this.timer
								
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.showNum = 2
								//提示关注成功
								this.$refs.guanzhu.open()
								if(res.data.status != 200) {
									console.log("出错了")
								}
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.topicInfoList = null
					}
				}catch (e) {
					console.log(e)
				}
			},
			//获取文章信息列表
			getTopicInfoList:function(){
				try{
					const token = uni.getStorageSync("tt")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'topic-info/list-TopicInfoList',
							data: {
								groupId:this.groupId
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.topicInfoList = res.data.result
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.topicInfoList = null
					}
				}catch (e) {
					console.log(e)
				}
			},
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
	.font1{
		font-size: 35upx;
		font-weight: bold;
	}
	.font2{
		font-size: 30upx;
		color: #808080;
	}
	.view1{
		display: flex;
		flex-direction: row;
		justify-content: center;
	}
	.view2{
		display: flex;
		width: 200upx;
		height: 60upx;
		border-radius: 10upx;
		line-height: 60upx;
		font-size: 30upx;
		justify-content: center;
		background-color: #ffaa00;
		color: #FFFFFF;
		border: 1upx solid #ffaa00;
	}
	.view3{
		display: flex;
		width: 200upx;
		height: 60upx;
		border-radius: 10upx;
		line-height: 60upx;
		font-size: 30upx;
		justify-content: center;
		background-color: #B2B2B2;
		color: #FFFFFF;
		border: 1upx solid #B2B2B2;
	}
	.view4{
		background-color: #333333;
		color: #FFFFFF;
		height: 60upx;
		padding-left: 20upx;
		padding-right: 20upx;
		font-size: 30upx;
		display: flex;
		justify-content: center;
		line-height: 60upx;
		border-radius: 10upx;
	}
</style>
