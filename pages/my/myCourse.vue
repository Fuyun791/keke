<template>
	<view>
		<!-- 顶部选项卡 -->
		<view class="layout1" style="padding-top: 20upx;">
			<view class="view1" :class="{view2:isActive1}" v-on:click="show(1)">全部</view>
			<view class="view1" :class="{view2:isActive2}" v-on:click="show(2)">课程</view>
			<view class="view1" :class="{view2:isActive3}" v-on:click="show(3)">活动</view>
		</view>
		<!-- 三个子版块 -->
		<!-- 全部版块 -->
		<view v-if="showNum == 1" style="margin-top: 50upx;">
			<!-- 全部中的课程 -->
			<uni-card isShadow="true" v-for="(item,index) in listarr1" :key="index">
				<view class="layout1">
					<view>
						<image style="width: 150upx;height: 150upx;border-radius: 20upx;" :src="item.url"></image>
					</view>
					<view>
						<view class="left1" style="font-weight: bold;">{{item.title}}</view>
						<view class="left1 top1 font1">{{item.name}}</view>
						<view class="left1 top2 font1">已学{{item.decimal}}课</view>
					</view>
				</view>
			</uni-card>
			<!-- 全部中的活动 -->
			<uni-card isShadow="true" v-for="(item,index) in listarr2" :key="index">
				<view class="layout1" style="height: 200upx;">
					<view>
						<image style="width: 150upx;height: 150upx;border-radius: 20upx;" :src="item.url"></image>
					</view>
					<view class="layout2 left1">
						<view style="font-weight: bold;">{{item.title}}</view>
						<view class="font1">时间:&nbsp;{{item.time}}</view>
						<view class="font1">地点:&nbsp;{{item.address}}</view>
						<view class="font1">发起人:&nbsp;{{item.name}}</view>
					</view>
				</view>
			</uni-card>
		</view>
		<!-- 课程版块 -->
		<view v-if="showNum == 2" style="margin-top: 50upx;">
			<uni-card isShadow="true" v-for="(item,index) in listarr1" :key="index">
				<view class="layout1">
					<view>
						<image style="width: 150upx;height: 150upx;border-radius: 20upx;" :src="item.url"></image>
					</view>
					<view>
						<view class="left1" style="font-weight: bold;">{{item.title}}</view>
						<view class="left1 top1 font1">{{item.name}}</view>
						<view class="left1 top2 font1">已学{{item.decimal}}课</view>
					</view>
				</view>
			</uni-card>
		</view>
		<!-- 活动版块 -->
		<view v-if="showNum == 3" style="margin-top: 50upx;">
			<uni-card isShadow="true" v-for="(item,index) in listarr2" :key="index">
				<view class="layout1" style="height: 200upx;">
					<view>
						<image style="width: 150upx;height: 150upx;border-radius: 20upx;" :src="item.url"></image>
					</view>
					<view class="layout2 left1">
						<view style="font-weight: bold;">{{item.title}}</view>
						<view class="font1">时间:&nbsp;{{item.time}}</view>
						<view class="font1">地点:&nbsp;{{item.address}}</view>
						<view class="font1">发起人:&nbsp;{{item.name}}</view>
					</view>
				</view>
			</uni-card>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isActive1:true,
				isActive2:false,
				isActive3:false,
				showNum: 1,
				listarr1:[
					{title:"计算机二级Office高级应用之...",name:"小黑课堂小黑老师",decimal:"1/25",url:'http://123.57.57.136/jklimage/XXX.png'},
					{title:"计算机二级Office高级应用之...",name:"小黑课堂小黑老师",decimal:"1/25",url:'http://123.57.57.136/jklimage/XXX.png'},
				],
				listarr2:[
					{title:"周末咖啡馆自律学习小组活动",time:"常年周末13:00-17:00",address:"好好读书大学东区咖啡馆",name:"好好读书Cate",url:'http://123.57.57.136/jklimage/1.jpg'},
					{title:"周末咖啡馆自律学习小组活动",time:"常年周末13:00-17:00",address:"好好读书大学东区咖啡馆",name:"好好读书Cate",url:'http://123.57.57.136/jklimage/1.jpg'},
				],
			}
		},
		methods: {
			show: function(num)
			{
				this.showNum = num;
				if(num == 1){
					this.isActive1 = true;
					this.isActive2 = false;
					this.isActive3 = false;
				}
				if(num == 2){
					this.isActive2 = true;
					this.isActive1 = false;
					this.isActive3 = false;
				}
				if(num == 3){
					this.isActive3 = true;
					this.isActive1 = false;
					this.isActive2 = false;
				}
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
		flex-direction: column;
		justify-content: space-around;
	}
	.left1{
		margin-left: 20upx;
	}
	.top1{
		margin-top: 10upx;
	}
	.top2{
		margin-top: 30upx;
	}
	.font1{
		color: #808080;
		font-size: 30upx;
	}
	.view1{
		width: 250upx;
		font-size: 35upx;
		text-align: center;
	}
	.view2{
		width: 250upx;
		font-size: 35upx;
		color: #eeb478;
		text-align: center;
	}
</style>
