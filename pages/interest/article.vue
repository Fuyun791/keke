<template>
	<view>
		<view style="padding: 20upx;" v-for="(item,index) in topicContentList" :key="index">
			<!-- 文章头部 -->
			<view style="font-weight: bold;font-size: 40upx;">{{item.topicName}}</view>
			<view class="layout1" style="margin-top: 20upx;">
				<view style="color: #2467cd;">{{item.groupName}}</view>
				<view style="color: #808080;margin-left: 10upx;">{{item.dataCreate}}</view>
			</view>
			<!-- 文章主体 -->
			<view style="margin-top: 20upx;" v-html="item.topicContent">{{item.topicContent}}</view>
			<view class="layout1" style="margin-top: 20upx;">
				<view style="color: #808080;">阅读</view>
				<view style="color: #808080;margin-left: 10upx;">33</view>
			</view>
		</view>
		<!-- 文章留言 -->
		<view style="padding: 20upx; margin-top: 20upx;background-color: #F1F1F1;padding-bottom: 40upx;">
			<view class="layout1" style="margin-top: 20upx;">
				<view style="color: #808080;width: 620upx;">精选留言</view>
				<view style="color: #2467cd;" @click="xieliuyan()">写留言</view>
			</view>
			<view class="layout1" style="margin-top: 40upx;width: 650upx;" v-for="(item,index) in groupCommentList" :key="index">
				<view>
					<image :src="item.adminPic" style="width: 70upx;height: 70upx;"></image>
				</view>
				<view class="layout2" style="margin-left: 10upx;">
					<view style="color: #808080;">{{item.studentName}}</view>
					<view>{{item.commentContent}}</view>
				</view>
			</view>
		</view>
		<!-- 文章留言板 -->
		<uni-popup ref="xieliuyan" type="bottom">
			<view style="width: 750upx;height: 1000upx;background-color: #FFFFFF;border-top-left-radius: 20upx;border-top-right-radius: 20upx;">
				<view class="layout1" style="padding: 20upx;">
					<view style="font-size: 30upx;padding: 10upx; background-color: #D3D3D3;color: #FFFFFF;border-radius: 5upx;" @click="quxiao()">取消</view>
					<view style="font-size: 35upx;margin-left: 195upx;padding: 10upx;">写留言</view>
					<view style="font-size: 30upx;margin-left: 230upx;padding: 10upx; background-color: #D3D3D3;color: #FFFFFF;border-radius: 5upx;" @click="tijiao()">提交</view>
				</view>
				<view style="padding: 20upx;font-size: 30upx;">
					<textarea v-model="commentContent" style="height: 250upx;width: 660upx; border: 1upx solid #808080;padding: 20upx;" placeholder="请输入您的评论,最多不超过50个字" adjust-position="false"maxlength="50">
						
					</textarea>
				</view>
			</view>
		</uni-popup>
		<!-- 提示评论成功 -->
		<uni-popup ref="pinglunchenggong" type="center">
			<view class="view4">评论成功~</view>
		</uni-popup>
		<!-- 提示未输入任何内容 -->
		<uni-popup ref="weishuru" type="center">
			<view class="view4">您还未输入任何内容!</view>
		</uni-popup>
	</view>
</template>

<script>
	import currentDate from "../../static/js/currentDate.js"
	
	export default {
		data() {
			return {
				topicId:"",
				topicContentList:[],
				groupCommentList:[],
				timer:"",
				commentContent:"",
			}
		},
		onLoad:function(option){
			this.topicId = option.id
			this.getTopicContentList()
			this.getGroupCommentList()
		},
		methods: {
			//点击写留言
			xieliuyan(){
				this.$refs.xieliuyan.open()
			},
			//点击提交
			tijiao(){
				//获取当前时间
				this.timer = currentDate.getDate()
				//判断内容是否为空
				if(this.commentContent == ""){
					//提示内容为空
					this.$refs.weishuru.open()
				}else{
					//执行插入一条评论
					this.insertGroupComment()
					//提示成功
					this.$refs.pinglunchenggong.open()
				}
				//关闭评论板
				this.$refs.xieliuyan.close()
			},
			//点击取消
			quxiao(){
				//关闭评论板
				 this.$refs.xieliuyan.close()
			},
			//插入一条评论
			insertGroupComment:function(){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'group-comment/insertGroupComment',
							data: {
								studentNum: studentNum,
								topicId: this.topicId,
								commentContent: this.commentContent,
								dataCreate: this.timer,
								dataModified: this.timer
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.getGroupCommentList()
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						console.log("您还没有登录")
					}
				}catch (e) {
					console.log(e)
				}
			},
			// 获取话题内容
			getTopicContentList:function(){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'topic-info/list-TopicContentList',
							data: {
								id: this.topicId
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.topicContentList = res.data.result
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.topicContentList = null
					}
				}catch (e) {
					console.log(e)
				}
			},
			//获取话题评论
			getGroupCommentList:function(){
				try{
					const token = uni.getStorageSync("tt")
					if (token) {
						uni.request({
							url:  getApp().globalData.requestUrl  + 'group-comment/list-GroupCommentList',
							data: {
								topicId: this.topicId
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.groupCommentList = res.data.result
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.groupCommentList = null
					}
				}catch (e) {
					console.log(e)
				}
			},
		}
	}
</script>

<style>
	view{
		font-size: 30upx;
	}
	.layout1{
		display: flex;
		flex-direction: row;
	}
	.layout2{
		display: flex;
		flex-direction: column;
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
