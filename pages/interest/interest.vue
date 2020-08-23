<template>
	<view>
		<!-- 顶部选项卡 -->
		<view class="layout2" style="padding-top: 20upx;padding-bottom: 20upx;">
			<button :class="{bottom3:isActive,bottom1:!isActive}" v-on:click="show(1)">智能推荐</button>
			<button :class="{bottom3:isActive2,bottom1:!isActive2}" v-on:click="show(2)">我的关注</button>
			<button :class="{bottom3:isActive3,bottom1:!isActive3}" v-on:click="show(3)">同校活动</button>
		</view>
		<!-- 三个功能模块 -->
		<!-- 智能推荐模块 -->
		<view v-if="showNum == 1" style="margin-top: 30upx;">
			<uni-card v-for="(item,index) in adviseGroupList" :key="index" isShadow="true">
				<!-- 第一块横向布局 -->
				<view class="layout1" @click="skip1(item.groupName,item.groupImg,item.id)">
					<view><image style="width: 100upx;height: 100upx;border-radius: 50%;" :src="item.groupImg"></image></view>
					<view style="margin-top: 10upx;">
						<view class="left1">{{item.groupName}}</view>
						<view class="left1 font2">{{item.groupBrief}}</view>
					</view>
				</view>
				<!-- 第二块横向布局 -->
				<view class="layout1" @click="skip2(item.topicInfo.id)">
					<view style="width:400upx;">
						<view class="font1">{{item.topicInfo.topicName}}</view>
						<view class="font2">{{item.topicInfo.topicBrief}}</view>
					</view>
					<view class="left1" style="margin-top: 10upx;">
						<image style="width: 200upx;height: 130upx;border-radius: 40upx;" :src="item.topicInfo.topicImg"></image>
						<view class="layout2">
							<view class="font2">499回复</view>
							<view class="font2">5分钟前</view>
						</view>
					</view>
				</view>
			</uni-card>
		</view>
		<!-- 我的关注模块 -->
		<view v-if="showNum == 2" style="margin-top: 30upx;">
			<uni-card isShadow="true" v-for="(item,index) in MyGroupList" :key="index">
				<view class="layout1">
					<view @click="skip1(item.groupName,item.groupImg,item.id)">
						<image style="width: 100upx;height: 100upx;border-radius: 50%; " :src="item.groupImg"></image>
					</view>
					<view style="margin-left: 20upx;width: 370upx;height: 120upx;" @click="skip1(item.groupName,item.groupImg,item.id)">
						<view>{{item.groupName}}</view>
						<view style="font-size: 30upx;color: #808080;">{{item.groupBrief}}</view>
					</view>
					<view style="margin-top: 30upx;">
						<view class="view3" @click="open(item.id)">取消关注</view>
					</view>
				</view>
			</uni-card>
		</view>
		<!-- 同校活动模块 -->
		<view v-if="showNum == 3" style="padding: 30upx;margin-top: 30upx;">
			<!-- 头部：我的学校 -->
			<view style="background-color: #F7F7F7;padding: 20upx;">
				<view class="layout1" style="background-color: #FFFFFF;padding: 20upx;">
					<view>我的学校:</view>
					<view class="left1" style="color: #ffaa00;">{{school}}</view>
				</view>
			</view>
			<!-- 主体：具体的同校活动 -->
			<view style="background-color: #F7F7F7;padding: 20upx;margin-top: 30upx;">
				<view class="layout1" style="background-color: #FFFFFF;padding: 20upx;margin-bottom: 30upx;" v-for="(item,index) in listarr3" :key="index">
					<view><image style="width: 160upx;height: 160upx;border-radius: 10upx;" :src="item.url"></image></view>
					<view class="layout3">
						<view class="left1" style="font-weight: bold;font-size: 32upx;">{{item.title}}</view>
						<view class="left1" style="font-size: 30upx;color: #808080;">时间:{{item.time}}</view>
						<view class="left1" style="font-size: 30upx;color: #808080;">地点:{{item.address}}</view>
						<view class="left1" style="font-size: 30upx;color: #808080;">发起人:{{item.promoter}}</view>
						<view>
							<button class="bottom2" style="margin-left: 230upx;margin-top: 20upx;">{{item.number}}人参加</button>
						</view>
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
				showNum : 1,
				isActive : true,
				isActive2 : false,
				isActive3 : false,
				school: "好好读书大学",
				listarr3:[
					{title:"周末咖啡馆自律学习小组活动",time:"常年周末13:00-17:00",address:"好好读书大学东区咖啡馆",promoter:"好好读书Cafe",number:"130",url:"http://123.57.57.136/jklimage/hu32.jpg"},
					{title:"周末咖啡馆自律学习小组活动",time:"常年周末13:00-17:00",address:"好好读书大学东区咖啡馆",promoter:"好好读书Cafe",number:"130",url:"http://123.57.57.136/jklimage/hu32.jpg"},
					{title:"周末咖啡馆自律学习小组活动",time:"常年周末13:00-17:00",address:"好好读书大学东区咖啡馆",promoter:"好好读书Cafe",number:"130",url:"http://123.57.57.136/jklimage/hu32.jpg"},
					{title:"周末咖啡馆自律学习小组活动",time:"常年周末13:00-17:00",address:"好好读书大学东区咖啡馆",promoter:"好好读书Cafe",number:"130",url:"http://123.57.57.136/jklimage/hu32.jpg"},
				],
				adviseGroupList:[],
				MyGroupList:[],
			}
		},
		onShow:function(){
			this.getAdviceGroupList()
			this.getMyGroupList()
		},
		methods: {
			//获取智能推荐小组列表
			getAdviceGroupList:function(){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'group-info/list-AdviceGroupList',
							data: {
								studentNum: studentNum
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.adviseGroupList = res.data.result
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.adviseGroupList = null
					}
				}catch (e) {
					console.log(e)
				}
			},
			//获取我的关注小组列表
			getMyGroupList:function(){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'group-info/list-MyGroupList',
							data: {
								studentNum: studentNum
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.MyGroupList = res.data.result
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						this.MyGroupList = null
					}
				}catch (e) {
					console.log(e)
				}
			},
			//取消关注一个小组
			getdeleteMyGroup:function(groupId){
				try{
					const token = uni.getStorageSync("tt")
					const studentNum = uni.getStorageSync("studentNum")
					if (token) {
						uni.request({
							url: getApp().globalData.requestUrl + 'group-info/deleteMyGroup',
							data: {
								studentNum: studentNum,
								groupId:groupId,
							},
							header: {
								"Authorization": "Bearer WX_" + token
							},
							success: res => {
								this.getMyGroupList()
							},
							fail: res => {
								console.log(res)
							}
						})
					}else{
						console('您还没有登录')
					}
				}catch (e) {
					console.log(e)
				}
			},
			//取消关注确认框
			open(groupId){
				uni.showModal({
				    title: '提示',
				    content: '你确定要取消关注吗?',
				    success: res => {
				        if (res.confirm) {
							this.getdeleteMyGroup(groupId)
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			},
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
			},
			skip1:function(title,url1,groupId){
				uni.navigateTo(
					{url:'group?title='+title+'&url1='+url1+'&groupId='+groupId}
				);
			},
			skip2:function(topicId){
				uni.navigateTo(
					{url:'article?id='+topicId}
				);
			},			
		},
	}
</script>

<style>
	view{
		font-size: 35upx;
	}
	.font1{
		font-size: 35upx;
		font-weight: bold;
	}
	.font2{
		font-size: 30upx;
		color: #808080;
	}
	.left1{
		margin-left: 20upx;
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
	.bottom1{
		border: 1upx solid #F8F1EC;
		border-radius: 200upx;
		width: 230upx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		background-color: #FFFFFF;
	}
	.bottom2{
		border-radius: 200upx;
		height: 60upx;
		line-height: 60upx;
		font-size: 30upx;
		background-color: #ffaa00;
		color: #FFFFFF;
	}
	.bottom3{
		border-radius: 200upx;
		width: 230upx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		color: #FFFFFF;
		background-color: #ffaa00;
	}
	.view3{
		display: flex;
		width: 150upx;
		height: 60upx;
		border-radius: 10upx;
		line-height: 60upx;
		font-size: 30upx;
		justify-content: center;
		background-color: #F0F0F0;
		color: #808080;
	}
	
</style>
