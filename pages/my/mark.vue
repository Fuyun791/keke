<template>
	<view>
		<view class="view1" style="font-size: 40upx;">CPA:{{CPAI}}</view>
		<view class="layout2 view2">
			<view style="width: 450upx">课程</view>
			<view style="width: 150upx">类型</view>
			<view style="width: 150upx">成绩</view>
		</view>
		<!-- 成绩的数据 -->
		<view class="layout2 view4" v-for="(item,index) in listarr1" :key="index">
			<view v-bind:class="{'view3' : index%2==0}" style="font-size: 30upx; width: 450upx">{{item.course}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="font-size: 30upx; width: 150upx">{{item.type}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="font-size: 30upx; width: 150upx">{{item.mark}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				CPAI : 4.489,			
				listarr1 : [
					{course:"计算机导论",type:"必修",mark:"78"},
					{course:"数据库原理",type:"必修",mark:"85"},
					{course:"JAVAEE程序设计",type:"必修",mark:"74"},
					{course:"JAVAEE程序设计实践",type:"必修",mark:"90"},
					{course:"Linux操作系统及应用",type:"限选",mark:"95"},
					{course:"网络安全技术",type:"限选",mark:"66"},
					{course:"就业指导2",type:"课外必修",mark:"77"},
					{course:"Html5移动开发",type:"选修",mark:"82"},
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
	.layout1{
		display: flex;
		flex-direction: row;
		justify-content: space-around;
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
