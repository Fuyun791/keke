<template>
	<view>
		<!-- 滚动模块 -->
		<view class="content">
			<special-banner :banner-list="bannerList" :swiper-config="swiperConfig"></special-banner>
		</view>
		<!-- 功能模块 -->
		<view class="menu">
			<view class="menu-in" @click="skip('course_list')">
				<image class="menu-img" src="../../static/icon/all-course.png"></image>
				<view>精品课程</view>
			</view>
			<view class="menu-in" @click="skip('library')">
				<image class="menu-img" src="../../static/icon/book.png"></image>
				<view>在线图书</view>
			</view>
			<view class="menu-in">
				<image class="menu-img" src="../../static/icon/live.png"></image>
				<view>名师直播</view>
			</view>
			<view class="menu-in" @click="skip('radio')">
				<image class="menu-img" src="../../static/icon/fm.png"></image>
				<view>FM电台</view>
			</view>
		</view>

		<!-- 推荐模块 -->
		<view class="card-backgroud">
			<view class="my-text" @click="skip">课程推荐</view>
			<view>
				<uni-list>
					<uni-list-item :showArrow="false"
					 v-for="(onlineCourse,index) in onlineCourseList" @click="selectOnlineCourse(onlineCourse)" :key="index">
						<view style="display: flex;">
							<view style="border-radius: 10rpx;padding-right: 9px;">
								<image :src="onlineCourse.onelinePic" class="course-list-image" style="border-radius: 8px;"></image>
							</view>
							<view class="course-list-right">
								<view class="course-name-text" v-text="onlineCourse.courseName"></view>
								<view class="course-author-text" v-text="onlineCourse.teacherName"></view>
								<view style="display: flex;align-items: center;height: 24px;width: 150px;margin-top: 14px;">
									<uni-rate disabled="true" :value="3.9"></uni-rate>
									<view class="rate-number">3.9</view>
								</view>
							</view>
						</view>
					</uni-list-item>
				</uni-list>
			</view>

			<!-- 每日一书 -->
			<view class="my-text">每日一书</view>
			<uni-card  v-for="(item,index) in listarr2" :key="index">
				<view class="card1">
					<view class="book-img">
						<image class="book-image" :src="item.url"></image>
					</view>
					<view>
						<view class="card-h1">{{item.title}}</view>
						<text class="book-title">{{item.name}}</text>
						<view class="book-title">{{item.brief}}</view>
						<view class="book-rate">
							<view class="book-fort">{{item.number}}</view>
							<uni-rate disabled="true" :value="item.number"></uni-rate>
						</view>
					</view>
				</view>
			</uni-card>

			<view class="my-text">每日一曲</view>
			<view style="margin: 12px;">
				<audio style="text-align: left;width: 320px;" :src="current.src" :poster="current.poster" :name="current.name" :author="current.author"
				 :action="audioAction" controls></audio>
			</view>

		</view>
	</view>
</template>

<script>
	import specialBanner from '../../components/EtherealWheat-banner/specialBanner.vue'
	import uniCard from '@/components/uni-card/uni-card.vue'
	export default {
		components: {
			specialBanner,
			uniCard
		},
		data() {
			return {
				onlineCourseList: [],
				current: {
					poster: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.jpg',
					name: '致爱丽丝',
					author: '暂无',
					src: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.mp3',
				},
				audioAction: {
					method: 'pause'
				},
				bannerList: [{
					picture: 'http://123.57.57.136/jklimage/2.jpg',
					title: '2020我们一起加油！',
					description: '该来的都在路上。',
					path: ''
				}, {
					picture: 'http://123.57.57.136/jklimage/8.jpg',
					title: '奔涌吧！后浪！',
					description: '千万不要让本来努力就可以得到的东西，因为懈怠而失去了机会。',
					path: ''
				}, {
					picture: 'http://123.57.57.136/jklimage/4.jpg',
					title: '清醒温柔知进退，努力上进且优秀',
					description: '在自己的小世界里，日日好日，夜夜好清宵',
					path: ''
				}],
				swiperConfig: {
					indicatorDots: true,
					indicatorColor: 'rgba(255, 255, 255, .4)',
					indicatorActiveColor: 'rgba(255, 255, 255, 1)',
					autoplay: true,
					interval: 5000,
					duration: 300,
					circular: true,
					previousMargin: '58rpx',
					nextMargin: '58rpx'
				},
				listarr2: [{
					title:"英语习语概论",
					name:"陈柏年",
					number: "4.5",
					brief:"英语习语，源远流长，数量浩繁，应用广泛。有的文人把他们比作“菜肴中的盐”，有的学者把它们喻为“食物中的维生素”，有的词书把它们称作“语言中的核心和精华”，有的专家甚至把它们视为“语言的生命和灵魂”。",
					url:'http://123.57.57.136/jklimage/book1.png',
				}],
			}

		},
		methods: {
			skip(name) {
				switch(name){
					case 'course_list':
						uni.navigateTo({
							url: 'course_list'
						});
						break;
					case 'radio':
						uni.navigateTo({
							url: '../radio/radio'
						})
						break;
					case 'library':
						uni.navigateTo({
							url: '../library/library'
						})
						break;
					default:
						return 0;
				}
			},
			selectOnlineCourse(onlineCourse) {
				uni.navigateTo({
					url: 'course?onlineCourse=' + encodeURIComponent(JSON.stringify(onlineCourse))
				})
			}
		},
		onShow: function() {
			try {
				const token = uni.getStorageSync("tt")
				if (token) {
					uni.request({
						url: getApp().globalData.requestUrl + 'online-course-info/list-online-course',
						data: {
							collegeId: uni.getStorageSync("collegeId"),
							isShare: 1,
							checkedStatus: 2,
							checkedResult: 1
						},
						header: {
							"Authorization": "Bearer WX_" + token
						},
						success: res => {
							if(res.data.status == 200) {
								this.onlineCourseList = res.data.result.list
							} else {
								uni.showToast({
									title: "登录状态过期请重新登录",
									duration: 2000
								})
								uni.clearStorage()
								uni.navigateTo({
									url: 'login'
								})
							}
						}
					})
				} else {
					uni.request({
						url: getApp().globalData.requestUrl + 'common/list-online-course',
						data: {
							isShare: 1,
							checkedStatus: 2,
							checkedResult: 1,
							pageStart: 1,
							pageSize: 10
						},
						success: (res) => {
							this.onlineCourseList = res.data.result.list
						}
					})
				}
			} catch (e) {
				console.log(e)
			}
		}
	}
</script>

<style>
	.course-list-image {
		width: 200rpx;
		height: 200rpx;
	}
	
	.course-context-text {
		font-size: 11pt;
	}
	
	.course-list-right {
		display: flex;
		flex-direction: column;
		padding: 4px;
	}
	
	.course-author-text {
		color: #b2b2b2;
		font-size: 10pt;
		padding: 4px 0px;
	}
	
	.course-name-text {
		font-weight: bold;
	}
	
	
	.content {
		width: 100vw;
		height: 550rpx;
		background-color: #FFFFFF;
	}

	.my-text {
		font-size: 14pt;
		font-weight: bold;
		margin-top: 20px;
	}

	.menu {
		margin-top: 5px;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		overflow: hidden;
	}
	
	.menu-in {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.menu-img {
		width: 28px;
		height: 28px;
		overflow: hidden;
		margin-bottom: 3px;
	}

	.card1 {
		display: flex;
		flex-direction: row-reverse;
		justify-content: space-around;

	}

	.card-backgroud {
		padding: 0px 12px;
	}

	.card-h1 {
		font-size: larg;
		font-weight: bold;
		width: 300rpx;
		height: 50rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 1;
		/* 限制在一个块元素显示的文本的行数 */
		-webkit-box-orient: vertical;
		/* 垂直排列 */
		word-break: break-all;
	}

	.card-h2 {
		font-size: 26rpx;
		color: #808080;
		margin-top: 10rpx;
		width: 300rpx;
		height: 50rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 1;
		/* 限制在一个块元素显示的文本的行数 */
		-webkit-box-orient: vertical;
		/* 垂直排列 */
		word-break: break-all;
	}

	.card-img {
		width: 225rpx;
		height: 225rpx;
		border-radius: 10rpx;
		overflow: hidden;
	}

	.card-image {
		width: 100%;
		height: 100%;
	}

	.card-rate {
		display: flex;
		flex-direction: row-reverse;
		justify-content: space-around;
		margin-top: 100rpx;
		height: 20rpx;
	}

	.rate-number {
		font-size: 28rpx;
		color: #999999;
		margin-left: 7px;
		height: 24px;
	}

	.book-img {
		width: 230rpx;
		height: 350rpx;
		border-radius: 10rpx;
		overflow: hidden;
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
		width: 300rpx;
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
		margin-top: 10rpx;
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
</style>
