<template>
	<view>
		<view class="view1" style="font-size: 60upx;">空教室</view>
		<view class="layout1 view2">
			<view>教室</view>
			<view>剩余座位号</view>
			<view>教室</view>
			<view>剩余座位数</view>
		</view>
		<!-- 教室的数据 -->
		<view class="layout2 view4" v-for="(item,index) in listarr1" :key="index">
			<view v-bind:class="{'view3' : index%2==0}" style="width: 165upx;">{{item.room1}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="width: 210upx;">{{item.number1}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="width: 165upx;">{{item.room2}}</view>
			<view v-bind:class="{'view3' : index%2==0}" style="width: 210upx;">{{item.number2}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listarr1 : [
					{room1:"B101",number1:"128",room2:"B106",number2:"101"},
					{room1:"B101",number1:"128",room2:"B106",number2:"101"},
					{room1:"B101",number1:"128",room2:"B106",number2:"101"},
					{room1:"B101",number1:"128",room2:"B106",number2:"101"},
					{room1:"B101",number1:"128",room2:"B106",number2:"101"},
					{room1:"B101",number1:"128",room2:"B106",number2:"101"}
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
