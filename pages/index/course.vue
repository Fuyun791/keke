<template>
	<view>
		<view style="height: 450upx;width: 100%; border-radius: 10upx;">
			<video style="width: 100%;" id="myVideo" :src="directorySrc" @error="videoErrorCallback" objectFit="contain"
			 controls></video>
		</view>
		<view class="layout3 left1 top1" style="height: 200upx;">
			<view style="font-weight: bold;" v-text="onlineCourse.courseName"></view>
			<view style="font-size: 30upx; color: #808080;" v-text="onlineCourse.teacherName"></view>
			<view>
				<view>评分4.9</view>
			</view>
		</view>
		<!-- 三个选项卡 -->
		<view class="layout2" style="padding-top: 20upx;padding-bottom: 20upx;">
			<button :class="{bottom3:isActive,bottom1:!isActive}" v-on:click="show(1)">简介</button>
			<button :class="{bottom3:isActive2,bottom1:!isActive2}" v-on:click="show(2)">课程</button>
			<button :class="{bottom3:isActive3,bottom1:!isActive3}" v-on:click="show(3)">评论</button>
		</view>

		<!-- 对应的三个页面 -->
		<!-- 简介 -->
		<view v-if="showNum == 1" style="margin-top: 10upx;">
			<!-- 适用人群 -->
			<view style="padding: 20upx;">
				大三学生
			</view>
			<!-- 课程概述 -->
			<view style="padding: 20upx;">
				<view style="font-weight: bold;">课程详情</view>
				<view v-text="onlineCourse.onlineBrief"></view>
			</view>
		</view>
		<!-- 目录 -->
		<view v-if="showNum == 2" style="padding: 20upx;">
			<view v-for="(episodes,index) in onlineEpisodes" :key="episodes.id">
				<view style="font-weight: bold;">
					第{{index + 1}}章 {{episodes.episodesName}} {{episodes.episodesBrief == null ? '': episodes.episodesBrief}}
				</view>
				<uni-card v-for="(courseHour,hourIndex) in episodes.onlineCourseHourList" :key="courseHour.id" isShadow="true"
				 @click="selectCourseHour(courseHour)">
					<view>课时{{hourIndex +1}} {{courseHour.courseHourName}} {{courseHour.courseHourBrief == null ? '' : courseHour.courseHourBrief}}</view>
					<view v-text="courseHour.hourTime != null ? courseHour.hourTime :'' "></view>
				</uni-card>
			</view>
		</view>
		<view v-if="showNum == 3">
			<template v-if="studentNum != ''">
				<view style="padding-bottom: 45px;">
					<view class="discuss" hover-class="discussToGray" v-for="(discuss,index) in onlineDiscuss" :key="index" @click="discussPersonThis(discuss.id,discuss.discussPersonName,discuss.discussPerson)">
						<!-- 头部 -->
						<view style="display: flex;align-items: center;">
							<view class="discuss-avator">
								<image style="width: 100%;height: 100%;" v-bind:class="{'discuss-no-avator': discuss.discussPersonPic == null}"
								 :src="discuss.discussPersonPic == null ? null : discuss.discussPersonPic"></image>
							</view>
							<view class="discuss-name">
								<view class="discuss-text" v-text="discuss.discussPersonName"></view>
								<view class="discuss-text" v-text="discuss.dataModified"></view>
							</view>
						</view>
						<!-- 评论文字 -->
						<view style="margin-left: 40px;">
							<view class="discuss-text" style="padding: 10px;" v-text="discuss.discussText"></view>
							<view class="discuss-context" v-if="discuss.onlineCourseDiscussList != null" :hover-stop-propagation="true" @click.stop="openDiscussIn(discuss.id,discuss.discussPerson)">
								<!-- 评论区 -->
								<view style="display: flex;margin-bottom: 5px;" 
								v-for="(discussTo,index) in discuss.onlineCourseDiscussList" 
								:key="discussTo.id" v-if="index <= 2">
									<view class="discuss-text" style="color: #007AFF;">
										{{discussTo.discussPersonName}}
										<template v-if="discussTo.discussToPersonName != null">
											<span style="padding: 0px 5px;color: #3B4144;">回复</span>
											{{discussTo.discussToPersonName}}
										</template>
										<!-- 这里有个 : 注意 -->
										：
									</view>
									<view class="discuss-text" v-text="discussTo.discussText"></view>
								</view>
								<view v-if="discuss.onlineCourseDiscussList.length > 3 " :hover-stop-propagation="true" @click.stop="openDiscussIn(discuss.id,discuss.discussPerson)">
									<view style="font-size: 9pt;color: #007AFF;">
										共{{discuss.onlineCourseDiscussList.length}}条回复
										<uni-icons type="arrowright" size="12" color="#007AFF"></uni-icons>
									</view>
								</view>
							</view>
							<view class="discuss-text" style="display: flex;align-items: center;">
								<view :hover-stop-propagation="true" @click.stop="clickGood(discuss.id,discuss.discussPerson,discuss)">
									<image :src="selectIsGood(discuss.id)" class="discuss-icon-small"></image>
								</view>
								<view class="discuss-button-text" style="color: #808080;margin-right: 14px;">{{discuss.star}}</view>
								<view :hover-stop-propagation="true" @click="discussPersonThis(discuss.id,discuss.discussPersonName,discuss.discussToPerson)">
									<image src="../../static/icon/discuss.png" class="discuss-icon-small"></image>
								</view>
							</view>
						</view>
					</view>
				</view>
				<!-- 底部栏 -->
				<view class="discuss-button" v-if="showDiscussContent">
					<view class="discuss-content" style="height: 45px;">
						<view>
							<image src="../../static/icon/forward.png" class="discuss-icon"></image>
						</view>
						<view class="discuss-button-text">转发</view>
					</view>
					<view class="discuss-content" style="height: 45px;" @click="openDiscuss">
						<view>
							<image src="../../static/icon/discuss.png" class="discuss-icon"></image>
						</view>
						<view class="discuss-button-text">评论</view>
					</view>
				</view>
				<uni-popup ref="popup" type="bottom">
					<view style="background-color: white;height: 55px;display: flex;align-items: center;z-index: 10;">
						<view style="width: 100%;padding: 7px 15px;">
							<input class="inputDiscuss" cursor-spacing="15" v-model="discussText" :focus="true"
							 :placeholder="discussPersonToName" />
						</view>
						<view style="width: 50px;font-size: 11pt;color: #808080;" @click="discussSumbit">发布</view>
					</view>
				</uni-popup>
			</template>
			<template v-else>
				<view style="display: flex;height: 100%;justify-content: center;align-items: center;padding-top: 40px;font-size: 11pt;">
					<span style="color: #808080;text-decoration: underline;" @click="toLogin">请先登录</span>
				</view>
			</template>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showNum: 1,
				isActive: true,
				isActive2: false,
				isActive3: false,
				onlineCourse: undefined,
				onlineEpisodes: [],
				onlineDiscuss: [],
				directorySrc: '',
				showDiscussContent: true,
				discussText: '',
				discussPersonToName: '说点什么吧',
				formData: {},
				studentNum: '',
				starList: []
			}
		},
		computed: {
			selectIsGood() {
				return function(id) {
					let goal = this.onlineCourse.id + "_" + id
					for(let star of this.starList) {
						if(star == goal) {
							return "../../static/icon/good-color.png"
						}
					}
					return "../../static/icon/good.png"
				}
			}
		},
		methods: {
			toLogin() {
				uni.navigateTo({
					url: 'login'
				})
			},
			discussSumbit() {
				if(this.discussText.length == 0) {
					return ;
				} 
				this.formData['discussText'] = this.discussText
				uni.request({
					url: getApp().globalData.requestUrl + 'online-course-discuss/add-discuss',
					method: 'POST',
					data: this.formData,
					header: {
						"Authorization": "Bearer WX_" + uni.getStorageSync("tt"),
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if(res.data.status == 200) {
							this.$refs.popup.close()
							uni.showToast({
								title: "评论成功",
								duration: 2000
							})
							this.discussText = ''
							this.getDicussList(this.onlineCourse)
						} else{
							uni.showToast({
								icon: 'none',
								title: "服务器出现了故障",
								duration: 2000
							})
						}
					}
				})
			},
			setDefaultForm() {
				// 留下了discussParent
				const formdata = {
					discussPerson: uni.getStorageSync("studentNum"),
					onlineCourseId: this.onlineCourse.id,
					discussPersonName: uni.getStorageSync("studentName"),
					discussPersonPic: uni.getStorageSync("studentAvatarUrl")
				}
				return formdata
			},
			openDiscuss() {
				this.discussPersonToName = '说点什么吧'
				let formdata = this.setDefaultForm()
				formdata['discussParent'] = 0
				this.formData = formdata
				this.$refs.popup.open()
			},
			discussPersonThis(discussParent,discussPersonToName,discussPerson) {
				this.discussPersonToName = '回复 @' + discussPersonToName + ':'
				let formdata = this.setDefaultForm()
				formdata['discussParent'] = discussParent
				formdata['discussToPersonId'] = discussPerson
				this.formData = formdata
				this.$refs.popup.open()
			},
			openDiscussIn(id,discussPerson) {
				let onlineCourseParam = {
					onlineCourseId: this.onlineCourse.id,
					id: id,
					discussPerson: discussPerson,
					pageStart: 1,
					pageSize: 20
				}
				uni.navigateTo({
					url: 'course-discuss?onlineCourseParam=' + encodeURIComponent(JSON.stringify(onlineCourseParam))
				})
			},
			setDefaultStarForm(id,discussPerson) {
				const starForm = {
					discussParent: 0,
					onlineCourseId: this.onlineCourse.id,
					starPerson: uni.getStorageSync("studentNum"),
					discussCourseId: id,
					discussToPerson: discussPerson
				}
				return starForm
			},
			isExisStar(id) {
				let goal = this.onlineCourse.id + "_" + id
				for(let index in this.starList) {
					if(this.starList[index] == goal) {
						return index
					}
				}
				return -1
			},
			clickGood(id,discussPerson,discuss) {
				let starForm = this.setDefaultStarForm(id,discussPerson)
				uni.request({
					url: getApp().globalData.requestUrl + 'online-course-discuss/click-star',
					method: 'POST',
					data: starForm,
					header: {
						"Authorization": "Bearer WX_" + uni.getStorageSync("tt"),
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if(res.data.status == 200) {
							let indexOf = this.isExisStar(id)
							uni.showToast({
								title: "点赞成功",
								duration: 2000
							})
							if(indexOf != -1) {
								discuss.star = discuss.star - 1
								this.starList.splice(indexOf,1)
							} else {
								discuss.star = discuss.star + 1
								this.starList.push(this.onlineCourse.id + "_" + id)
							}
						} else{
							uni.showToast({
								icon: 'none',
								title: "服务器出现了故障",
								duration: 2000
							})
						}
					}
				})
			},
			show: function(num) {
				this.showNum = num;
				if (num == 1) {
					this.isActive = true;
					this.isActive2 = false;
					this.isActive3 = false;
				}
				if (num == 2) {
					this.isActive2 = true;
					this.isActive = false;
					this.isActive3 = false;
				}
				if (num == 3) {
					this.isActive3 = true;
					this.isActive = false;
					this.isActive2 = false;
				}
			},
			videoErrorCallback(e) {
				uni.showModal({
					content: e.target.errMsg,
					showCancel: false
				})
			},
			selectCourseHour(value) {
				this.directorySrc = value.directory
			},
			getDicussList(onlineCourse) {
				uni.request({
					url: getApp().globalData.requestUrl + 'online-course-discuss/list-star',
					data: {
						onlineCourseId: onlineCourse.id,
						discussPerson: uni.getStorageSync("studentNum")
					},
					header: {
						"Authorization": "Bearer WX_" + uni.getStorageSync("tt")
					},
					success: res => {
						this.starList = res.data.result
						uni.request({
							url: getApp().globalData.requestUrl + 'online-course-discuss/list-discuss',
							data: {
								onlineCourseId: onlineCourse.id
							},
							header: {
								"Authorization": "Bearer WX_" + uni.getStorageSync("tt")
							},
							success: res => {
								this.onlineDiscuss = res.data.result
							}
						})
					}
				})
			},
			getEpisodesList(onlineCourse) {
				const token = uni.getStorageSync("tt")
				if (token) {
					uni.request({
						url: getApp().globalData.requestUrl + 'online-course-info/list-episodes-hour',
						data: {
							onlineCourseId: onlineCourse.id,
							collegeId: uni.getStorageSync("collegeId")
						},
						header: {
							"Authorization": "Bearer WX_" + uni.getStorageSync("tt")
						},
						success: res => {
							let epiosded = res.data.result
							this.onlineEpisodes = epiosded
							if (epiosded.length > 0) {
								this.directorySrc = epiosded[0].onlineCourseHourList[0].directory
							}
						}
					})
				} else {
					// 加一个公共方法来实现，在mapper处sql加上对课程的判断
					uni.request({
						url: getApp().globalData.requestUrl + 'common/list-episodes-hour',
						data: {
							onlineCourseId: onlineCourse.id
						},
						success: res => {
							let epiosded = res.data.result
							console.log(res)
							this.onlineEpisodes = epiosded
							if (epiosded.length > 0) {
								this.directorySrc = epiosded[0].onlineCourseHourList[0].directory
							}
						}
					})
				}
			}
		},
		onShow: function() {
			this.studentNum = uni.getStorageSync("studentNum")
			this.getEpisodesList(this.onlineCourse)
			this.getDicussList(this.onlineCourse)
		},
		// 如果是回到首页再进入会再触发，而和它一样的二级页面进入不会触发
		onLoad: function(option) {
			const onlineCourse = JSON.parse(decodeURIComponent(option.onlineCourse))
			this.onlineCourse = onlineCourse
		}
	}
</script>

<style>
	.discuss {
		border-top: 1px solid #e5e9ef;
		display: flex;
		flex-direction: column;
		padding: 15px 8px;
	}
	
	.discussToGray {
		background-color: #e5e5e5;
	}
	
	.uni-popup.data-v-aa7bb0d4 {
		bottom: 100px;
	}
	
	.inputDiscuss {
		display: flex;
		padding: 5px 8px 5px 15px;
		border-width: 0.5px;
		border-style: solid;
		border-radius: 100px;
		background-color: #F8F8F8;
		border-color: #e5e5e5;
		align-items: center;
		font-size: 10pt;
	}

	.discuss-button {
		position: fixed;
		bottom: 0px;
		width: 100%;
		height: 45px;
		z-index: 2;
		color: #808080;
		display: flex;
		align-items: center;
		justify-content: space-around;
		background-color: white;
	}

	.discuss-icon {
		width: 17px;
		height: 17px;
		margin-right: 5px;
	}
	
	.discuss-icon-small {
		width: 14px;
		height: 14px;
		margin-right: 5px;
	}

	.discuss-content {
		display: flex;
		align-items: center;
	}

	.discuss-button-text {
		font-size: 10pt;
	}

	.discuss-avator {
		width: 40px;
		height: 40px;
		border-radius: 50%;
		overflow: hidden;
	}

	.discuss-no-avator {
		background-color: #007AFF;
	}

	.discuss-context {
		background-color: #f3f3f4;
		border-radius: 2px;
		padding: 10px;
	}

	.discuss-text {
		font-size: 14px;
	}

	.discuss-name {
		margin-left: 10px;
		color: #808080;
	}

	view {
		font-size: 35upx;
	}

	.layout2 {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}

	.layout3 {
		display: flex;
		flex-direction: column;
		justify-content: space-around;
	}

	.left1 {
		margin-left: 20upx;
	}

	.top1 {
		margin-top: 30upx;
	}

	.bottom1 {
		border-radius: 10upx;
		width: 230upx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		color: #CCCCCC;
	}

	.bottom3 {
		border-radius: 10upx;
		width: 230upx;
		height: 70upx;
		line-height: 70upx;
		font-size: 35upx;
		color: #FFFFFF;
		background-color: #646566;
		border-color: #FFFFFF;
	}
</style>
