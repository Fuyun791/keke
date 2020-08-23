<template>
	<view>
		<view class="view1" style="font-size: 40upx;">我的考试</view>
		<view class="layout2 view2">
			<view style="width: 250upx">课程名称</view>
			<view style="width: 250upx">考试时间</view>
			<view style="width: 250upx">考试地点</view>
		</view>
		<!-- 我的考试数据 -->
		<view class="layout2 view4" v-for="(item,index) in listarr1" :key="index">
			<view v-bind:class="{'view3' : index%2==0}" style="font-size: 30upx; width: 250upx">{{item.course}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="font-size: 25upx; width: 250upx">{{item.time}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="font-size: 30upx; width: 250upx">{{item.room}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listarr1 : [
					{course:"计算机导论",time:"6月26日(13:00-15:00)",room:"E301"},
					{course:"数据库原理",time:"6月26日(13:00-15:00)",room:"E301"},
					{course:"JAVAEE程序设计",time:"6月26日(13:00-15:00)",room:"E301"},
					{course:"就业指导2",time:"6月26日(13:00-15:00)",room:"E301"},
					{course:"Html5移动开发",time:"6月26日(13:00-15:00)",room:"E301"},
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	view{
		font-size: 35upx;
	}
	.layout2{
		display: flex;
		flex-direction: row;
	}
	.view1{
		background-color: #eeb478;
		height: 250upx;
		color: #FFFFFF;
		text-align: center;
		line-height: 250upx;
	}
	.view2{
		height: 100upx;
		background-color: #f0f0f0;
		color: #808080;
		text-align: center;
		line-height: 100upx;
	}
	.view3{
		height: 100upx;
		background-color: #f8f1ec;
		text-align: center;
		line-height: 100upx;
	}
	.view4{
		height: 100upx;
		background-color: #FFFFFF;
		text-align: center;
		line-height: 100upx;
	}
</style>
