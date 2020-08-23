<template>
	<view>
		<view class="search-input ">
			<input class="search-box" type="text" v-model="search" >
		</view>
		<view>
			<view class="btn-row" style="padding-top: 20upx;padding-bottom: 20upx;">
				<view :class="{bottom:isActive,bottom1:!isActive}" v-on:click="show(1)">推荐</view>
				<view :class="{bottom:isActive2,bottom1:!isActive2}" v-on:click="show(2)">新书</view>
				<view :class="{bottom:isActive3,bottom1:!isActive3}" v-on:click="show(3)">热评</view>
				<view :class="{bottom:isActive4,bottom1:!isActive4}" v-on:click="show(4)">畅销</view>
				<view :class="{bottom:isActive5,bottom1:!isActive5}" v-on:click="show(5)">免费</view>
			</view>
		</view>
		<view v-if="showNum == 1" style="margin-top: 10rpx; margin-right: 20rpx;" >
				<uni-list >
				<uni-list-item v-for="(item,index) in listarr" :key="index" >
					<view style="display: flex;" >
						<view style="border-radius: 10rpx;padding-right: 9px;" >
							<image :src="item.url" class="course-list-image"></image>
						</view>
						<view class="course-list-right"  @click="skip1(item.title,item.name,item.brief,item.url)">
							<view style="padding-top: 40rpx">
							<view class="course-name-text">{{item.title}}</view>
							<view class="course-author-text">{{item.name}}</view>
							<view class="course-context-text">{{item.brief}}</view>
							</view>
						</view>
					</view>
					
				</uni-list-item>
			</uni-list>
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
				isActive4 : false,
				isActive5 : false,
				
				
				listarr:[
					{title:"英语习语概论",name:"陈柏年",brief:"英语习语，源远流长，数量浩繁，应用广泛。有的文人把他们比作“菜肴中的盐”，有的学者把它们喻为“食物中的维生素”，有的词书把它们称作“语言中的核心和精华”，有的专家甚至把它们视为“语言的生命和灵魂”。",url:'http://123.57.57.136/jklimage/book1.png'},
					{title:"英语衍生句法",name:"林天明",brief:"语法就是语言的规律。语言的规律，应该到语言里去找。可是，人类对语言的认识，并不是一下子就这么深入的",url:'http://123.57.57.136/jklimage/book2.png'},
					{title:"请教亚历山大",name:"亚历山大",brief:"怎样才能学好英语？英语有什么特点？怎样看待学习中的语法现象？",url:'http://123.57.57.136/jklimage/book3.png'},
					{title:"翻译-思考与试笔",name:"王佐良 ",brief:"从文学的角度论述了若干的翻译问题及背后的文化差异问题",url:'http://123.57.57.136/jklimage/book4.png'},
					{title:"翻译-思考与试笔",name:"王佐良 ",brief:"从文学的角度论述了若干的翻译问题及背后的文化差异问题",url:'http://123.57.57.136/jklimage/XXX.png'},   
				],
				
			}
		},
		methods: {
			show: function(num){
				this.showNum = num;
					if(num == 1){
						this.isActive =  true;
						this.isActive2 = false;
						this.isActive3 = false;
						this.isActive4 = false;
						this.isActive5 = false;
					}
					if(num == 2){
					this.isActive2 = true;
					this.isActive = false;
					this.isActive3 = false;
					this.isActive4 = false;
					this.isActive5 = false;
					}
					if(num == 3){
						this.isActive3 = true;
						this.isActive = false;
						this.isActive2 = false;
						this.isActive4 = false;
						this.isActive5 = false;
					}
					if(num == 4){
						this.isActive4 = true;
						this.isActive = false;
						this.isActive2 = false;
						this.isActive3 = false;
						this.isActive5 = false;
					}
					if(num == 5){
						this.isActive5 = true;
						this.isActive = false;
						this.isActive2 = false;
						this.isActive3 = false;
						this.isActive4 = false;
					}
					
			},
			skip1:function(title,name,breif,url){
			uni.navigateTo(
				{url:"./book?title="+title+"&name="+name+"&breif="+breif+"&url="+url}
			);
		},
		},
		
	}
</script>

<style>
	.course-list-image {
		width: 200rpx;
		height: 250rpx;
	}
	
	.course-context-text {
		font-size: 11pt;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		/* 限制在一个块元素显示的文本的行数 */
		-webkit-box-orient: vertical;
		/* 垂直排列 */
		word-break: break-all;
	}
	
	.course-list-right {
		display: flex;
		flex-direction: column;
	}
	
	.course-author-text {
		color: #b2b2b2;
		font-size: 11pt;
		padding: 4px 0px;
	}
	
	.course-name-text {
		font-weight: bold;
	}
	.search-input{
		height: 70rpx;
		width: 700rpx;
		margin: 20rpx;
		border-radius: 50rpx;
		border:1px solid #C0C0C0;
	}
	.search-box{
		height: 70rpx;
		margin: 0 auto;
		width: 600rpx;
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
		text-align: center;
	}
	
	.bottom{
		width: 250upx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		background-color: #FFFFFF;
		color: #ffaa00;
		text-align: center;
	}
</style>
