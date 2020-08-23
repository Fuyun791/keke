<template>
	<view>
			<view class="card1">
				<view class="book-img">
					<image class="book-image" :src="url"></image>
				</view>
					<view style="padding-top: 50rpx;">
						<view class="card-h1" style="font-weight: bold; font-size: larger;">{{title}}</view>
						<text class="book-title" style="color: #55AAFF; font-weight: bold;">{{name}}</text>
						<view></view>
						<view class="book-title">{{breif}}</view>
					</view>
			</view>	
			<view style="display: flex;height: 100rpx; padding: 50rpx;">
				<view style="margin-top: 20rpx;">
				<uni-rate value="5" disabled="false"></uni-rate>
				<view style="margin-top: 10rpx; font-size: small;color: #AAAAAA">10人点评</view>
				</view>
				
				<view style="margin-left: 50rpx;">
						<view style="color: #55AAFF; font-weight: bold;">45人</view>
						<view style="font-size: small;color: #AAAAAA">在读此书</view>
				</view>
				
				<button class="follow">
				                    <view class="true">收藏</view>
				                </button>
			</view>
			
			<view >
				<text style="padding-left: 20px;">目录</text>
				<uni-list>
					<uni-list-item v-for="(item,index) in listarr" :key="index" >
						<text>{{item.title}}</text>
					</uni-list-item>
				</uni-list>
				
			</view>
			
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
			title:"",
			name:"",
			breif:"",
			url:"",
			
			listarr:[
				{title:"序言",},
				{title:"第一章",},
				{title:"第二章",},
				{title:"第三章",},
				{title:"第四章",},
				{title:"第五章",},
				{title:"结语",},
			],
			}
		},
		onLoad(option) {
			uni.setNavigationBarTitle({
				title:option.title,
			})
			this.url = option.url,
			this.title = option.title,
			this.name = option.name,
			this.breif = option.breif		
		},
		
		
		
		methods: {
		
		}
	}
</script>

<style>
	.card1 {
			display: flex;
			flex-direction: row;
			margin-top: 50rpx;
			padding-left: 30rpx;
	}
	
	.card-h1 {
		font-size: larg;
		font-weight: bold;
		width: 300rpx;
		height: 50rpx;
		margin-left: 50rpx;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		
	}
		
	.book-img {
			width: 250rpx;
			height: 350rpx;
			border-radius: 10rpx;
	}

	.book-image {
		width: 100%;
		height: 100%;
		padding-bottom: 100%;
		overflow: hidden;
	}

	.book-title {
		font-size: 26rpx;
		color: #808080;
		margin-top: 10rpx;
		margin-left: 50rpx;
		width: 400rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 5;
		/* 限制在一个块元素显示的文本的行数 */
		-webkit-box-orient: vertical;
		/* 垂直排列 */
		word-break: break-all;
	}

	.book-fort {
		font-size: 32rpx;
		color: #808080;
		width: 50rpx;
		height: 50rpx;
		overflow: hidden;
	}

	.book-rate {
		margin-top: 50rpx;
		display: flex;
		flex-direction: row-reverse;
		justify-content: space-around;
	}
	
	.follow{
		display: flex;
		width: 200upx;
		height: 60upx;
		border-radius: 10upx;
		line-height: 60upx;
		font-size: 30upx;
		justify-content: center;
		background-color: #EEB478;
		color: #FFFFFF;
		position: fixed;
		right: 20rpx;
	}

</style>
