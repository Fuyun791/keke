<template>
	<view>
		<!-- 头部选项卡 -->
		<view class="layout1" style="padding-top: 20upx;">
			<view class="view1" :class="{view2:isActive1}" v-on:click="show(1)">全部</view>
			<view class="view1" :class="{view2:isActive2}" v-on:click="show(2)">书籍</view>
			<view class="view1" :class="{view2:isActive3}" v-on:click="show(3)">文章</view>
			<view class="view1" :class="{view2:isActive4}" v-on:click="show(4)">电台</view>
		</view>
		<!-- 四个子版块 -->
		<!-- 全部版块 -->
		<view v-if="showNum == 1" style="margin-top: 50upx;">
			<!-- 全部中的书籍 -->
			<uni-card isShadow="true" v-for="(item,index) in listarr1" :key="index">
				<view class="layout1">
					<view><image style="width: 180upx;height: 240upx;border-radius: 10upx; " :src="item.url"></image></view>
					<view class="layout3" style="margin-left: 20upx;">
						<view style="font-weight: bold;">{{item.title}}</view>
						<view class="font1">{{item.name}}</view>
						<view>评分:{{item.number}}</view>
						<view class="font1">{{item.brief}}</view>
					</view>
				</view>
			</uni-card>
			<!-- 全部中的文章 -->
			<uni-card isShadow="true" v-for="(item,index) in listarr2" :key="index">
				<view style="font-weight: bold;">{{item.title}}</view>
				<view class="font1">{{item.brief}}</view>
			</uni-card>
		</view>
		<!-- 书籍版块 -->
		<view v-if="showNum == 2" style="margin-top: 50upx;">
			<uni-card isShadow="true" v-for="(item,index) in listarr1" :key="index">
				<view class="layout1">
					<view><image style="width: 180upx;height: 240upx;border-radius: 10upx; " :src="item.url"></image></view>
					<view class="layout3" style="margin-left: 20upx;">
						<view style="font-weight: bold;">{{item.title}}</view>
						<view class="font1">{{item.name}}</view>
						<view>评分:{{item.number}}</view>
						<view class="font1">{{item.brief}}</view>
					</view>
				</view>
			</uni-card>
		</view>
		<!-- 文章版块 -->
		<view v-if="showNum == 3" style="margin-top: 50upx;">
			<uni-card isShadow="true" v-for="(item,index) in listarr2" :key="index">
				<view style="font-weight: bold;">{{item.title}}</view>
				<view class="font1">{{item.brief}}</view>
			</uni-card>
		</view>
		<!-- 电台版块 -->
		<view v-if="showNum == 4" style="margin-top: 50upx;">
			<uni-card isShadow="true">
				<view class="layout1">
					<view style="background-image: url(http://123.57.57.136/jklimage/hu31.jpg);width: 180upx;height: 180upx;border-radius: 10upx;">
						<view class="layout1" style="padding: 10upx;margin-top: 120upx;">
							<view><image style="width: 40upx;height: 40upx;" src="http://123.57.57.136/jklimage/hu30.jpg"></image></view>
							<view style="font-size: 30upx;color: #FFFFFF;">100收听</view>
						</view>
					</view>
					<view class="layout3" style="margin-left: 20upx;">
						<view style="font-size: 35upx;">小明说故事</view>
						<view style="font-size: 30upx;color: #808080;">第10集(完)</view>
						<view style="font-size: 30upx;color: #808080;">小明</view>
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
				isActive1 : true,
				isActive2 : false,
				isActive3 : false,
				isActive4 : false,
				showNum : 1,
				listarr1 : [
					{title:"海风中失落的血色馈赠",
					 name:"[加拿大]阿丽斯泰尔.麦克劳德",
					 number:"4.9",
					 brief:"因为我们知道，那些深埋到眼泪都融发不了的情感，并不是只有能说会道的人才有。",
					 url:'http://123.57.57.136/jklimage/hu18.jpg'},
					 {title:"百年孤独",
					  name:"[哥伦比亚]加西亚.马尔克斯",
					  number:"5.0",
					  brief:"人们一派懈怠，而遗忘却日渐贪婪，无情地吞噬一点一滴的记忆",
					  url:'http://123.57.57.136/jklimage/hu19.jpg'},
				],
				listarr2 : [
					{title: "不亚于陀思妥耶夫斯基的数学",
					 brief: "宫崎学长预约巴黎93省Bobigny长居3个月以后，终于来到了警察局的27号窗口前，整个流程在1分钟内结束，像是他从日超买好了食材，花了3个小时准备丰盛的料理，然后一个人5分钟内吃完了便..."},
					{title: "民间笑话荟萃",
					 brief: "妻子突然问丈夫：“你爱我吗？” “爱，当然爱！”丈夫毫不犹豫地回答。 妻子想了想又问：“你是不是怕伤害我才说的？” 丈夫连忙说：“不，不，我是怕你伤害我才说的..."},
				]
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
					this.isActive4 = false;
				}
				if(num == 2){
					this.isActive2 = true;
					this.isActive1 = false;
					this.isActive3 = false;
					this.isActive4 = false;
				}
				if(num == 3){
					this.isActive3 = true;
					this.isActive1 = false;
					this.isActive2 = false;
					this.isActive4 = false;
				}
				if(num == 4){
					this.isActive4 = true;
					this.isActive1 = false;
					this.isActive2 = false;
					this.isActive3 = false;
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
	.layout3{
		display: flex;
		flex-direction: column;
		justify-content: space-around;
	}
	.font1{
		font-size: 30upx;
		color: #808080;
	}
	.view1{
		width: 187.5upx;
		font-size: 35upx;
		text-align: center;
	}
	.view2{
		width: 187.5upx;
		font-size: 35upx;
		color: #eeb478;
		text-align: center;
	}
</style>
