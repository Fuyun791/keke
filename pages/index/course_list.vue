<template>
	<view>
		<view @click="switchSearch">
			<!-- 搜索框 -->
			<uni-search-bar :radius="100" placeholder="请输入课程名称" @confirm="searchSumbit" @input="searchInput"></uni-search-bar>
		</view>
		<view style="height: 7px;width: 100%;background-color: #F7F7F7;"></view>
		<view>
			<scroll-view class="nav" scroll-x>
				<view @click="activePage(index)" class="nav_item" :class="{active: index == currentIndex}" v-for="(tag,index) in tagList">
					{{tag}}
				</view>
			</scroll-view>
		</view>
		<view>
			<uni-list>
				<uni-list-item :showArrow="false"
				 v-for="(item, index) in onlineCourseList" :key="index" @click="selectOnlineCourse(item)">
					<view style="display: flex;">
						<view style="border-radius: 10rpx;padding-right: 9px;">
							<image :src="item.onelinePic" class="course-list-image" style="border-radius: 8px;"></image>
						</view>
						<view class="course-list-right">
							<view class="course-name-text" v-text="item.courseName"></view>
							<view class="course-author-text" v-text="item.teacherName"></view>
							<view class="course-context-text" v-text="item.onlineBrief"></view>
						</view>
					</view>
				</uni-list-item>
			</uni-list>
		</view>
	</view>
</template>

<script>
	import uniSearchBar from '@/components/uni-search-bar/uni-search-bar.vue'
	export default {
		components: {
			uniSearchBar,
		},
		data() {
			return {
				search: '',
				currentIndex: 0,
				tagList: ['全部','java', '高等数学', '应用写作', '软件测试', '英语阅读', '英语写作'],
				onlineCourseList: []
			}
		},

		computed: {
			//过滤方法
			items: function() {
				var _search = this.search;
				if (_search) {
					//不区分大小写处理
					var reg = new RegExp(_search, 'ig')
					//es6 filter过滤匹配，有则返回当前，无则返回所有
					return this.listarr1.filter(function(e) {
						//匹配所有字段
						// return Object.keys(e).some(function(key) {
						//     return e[key].match(reg);
						// })
						//匹配某个字段
						return e.title.match(reg);
					})
				};
				return this.listarr1;
			}
		},
		methods: {
			selectOnlineCourse(onlineCourse) {
				uni.navigateTo({
					url: 'course?onlineCourse=' + encodeURIComponent(JSON.stringify(onlineCourse))
				})
			},
			activePage(index) {
				this.currentIndex = index
			},
			switchSearch() {
				console.log(8)
			},
			searchSumbit(e) {
				console.log(e)
			},
			searchInput(e) {
				console.log(e)
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
							this.onlineCourseList = res.data.result.list
						},
						fail: res => {
							console.log(res)
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
		},
		onLoad: function() {
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

	.nav {
		white-space: nowrap;
		padding: 5rpx 0;
	}

	.nav_item {
		padding: 20rpx 40rpx;
		font-size: 10pt;
		display: inline-block;
	}

	.nav_item.active {
		border-bottom: 4rpx solid palevioletred;
	}

	.search-input {
		height: 70rpx;
		width: 700rpx;
		margin: 20rpx;
		border-radius: 50rpx;
		border: 1px solid #C0C0C0;
	}

	.search-box {
		height: 70rpx;
		margin: 0 auto;
		width: 600rpx;
	}

	.h1 {
		margin-left: 25rpx;
		margin-top: 5rpx;
		margin-bottom: 5rpx;
		font-size: x-large;
		font-weight: bold;
		overflow: hidden;

	}

	.card {
		margin-left: 20rpx;
		margin-right: 20rpx;
		border-radius: 10rpx;
	}

	.card1 {
		display: flex;
		flex-direction: row-reverse;
		justify-content: space-around;

	}

	.card-backgroud {
		padding-top: 20rpx;
		padding-left: 20rpx;
		padding-right: 20rpx;

		background-color: #F7F7F7;
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
		width: 250rpx;
		height: 250rpx;
		border-radius: 10rpx;
		overflow: hidden;
	}

	.card-image {
		width: 100%;
		height: 100%;
		padding-bottom: 100%;
		overflow: hidden;
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
	}
</style>
