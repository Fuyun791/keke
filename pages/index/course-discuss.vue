<template>
	<view v-if="onlineCourse != null">
		<view class="discuss" v-if="onlineCourse != undefined" 
		hover-class="discussToGray" 
		@click="discussPersonThis(onlineCourse.id,onlineCourse.discussPersonName,onlineCourse.discussPerson)">
			<!-- 头部 -->
			<view style="display: flex;align-items: center;">
				<view class="discuss-avator">
					<image style="width: 100%;height: 100%;"
					 v-bind:class="{'discuss-no-avator': onlineCourse.discussPersonPic == null}"
					 :src="onlineCourse.discussPersonPic == null ? null : onlineCourse.discussPersonPic"></image>
				</view>
				<view class="discuss-name">
					<view class="discuss-text" v-text="onlineCourse.discussPersonName"></view>
					<view class="discuss-text" v-text="onlineCourse.dataModified"></view>
				</view>
			</view>
			<!-- 评论文字 -->
			<view style="margin-left: 40px;">
				<view class="discuss-text" style="padding: 10px;" 
				v-text="onlineCourse.discussText"></view>
				<view class="discuss-text"
				 style="display: flex;align-items: center;justify-content: space-between;">
				 <view class="discuss-context-left">
					 <view :hover-stop-propagation="true"
					  @click.stop="clickFirstGood(onlineCourse.discussParent,onlineCourse.id,onlineCourse.discussPerson,onlineCourse)">
					 	<image :src="selectIsGood(onlineCourse.id)" class="discuss-icon-small"></image>
					 </view>
					 <view class="discuss-button-text" style="color: #808080;margin-right: 14px;">{{onlineCourse.star}}</view>
					 <view :hover-stop-propagation="true"
					  @click="discussPersonThis(onlineCourse.id,onlineCourse.discussPersonName,onlineCourse.discussToPerson)">
					 	<image src="../../static/icon/discuss.png" class="discuss-icon-small"></image>
					 </view>
				 </view>
				 <view class="discuss-context-right" v-if="showEdit(onlineCourse.discussPerson)">删除</view>
				</view>
			</view>
		</view>
		<view style="height: 7px;width: 100%;background-color: #F7F7F7;"></view>
		<view class="discuss" hover-class="discussToGray"
		 v-for="(discuss,index) in onlineCourse.onlineCourseDiscussList"
		 :key="index" @click="replyDiscuss(discuss.discussParent,onlineCourse.discussPerson,discuss.discussPerson,discuss.discussPersonName)">
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
			<view style="margin-left: 40px;">
				<view class="discuss-text" style="color: #007AFF;display: flex;margin-bottom: 8px;">
					<template v-if="discuss.discussToPersonName != null">
						<span style="padding: 0px 5px;color: #3B4144;">回复</span>
						{{discuss.discussToPersonName}}
						<!-- 这里有个 : 注意 -->
						：
					</template>
					<view class="discuss-text" style="color: #3B4144;" v-text="discuss.discussText"></view>
				</view>
				<view class="discuss-text" style="display: flex;align-items: center;justify-content: space-between;">
					<view class="discuss-context-left">
						<view :hover-stop-propagation="true"
						 @click.stop="clickGood(discuss.discussParent,discuss.id,onlineCourse.discussPerson,discuss.discussPerson,discuss)">
							<image :src="selectIsGood(discuss.id)" class="discuss-icon-small"></image>
						</view>
						<view class="discuss-button-text" style="color: #808080;margin-right: 14px;">{{discuss.star}}</view>
						<view :hover-stop-propagation="true"
						 @click="replyDiscuss(discuss.discussParent,onlineCourse.discussPerson,discuss.discussPerson,discuss.discussPersonName)">
							<image src="../../static/icon/discuss.png" class="discuss-icon-small"></image>
						</view>
					</view>
					<view class="discuss-context-right" v-if="showEdit(discuss.discussPerson)">删除</view>
				</view>
			</view>
		</view>
		<view class="discuss-button" :style="{bottom: inputBottom + 'px'}">
			<view class="discuss-button-in">
				<view style="width: 100%;padding: 7px 15px;">
					<input class="inputDiscuss" cursor-spacing="15" :adjust-position="false" @focus="inputFocus" @blur="inputBlur" v-model="discussText" :placeholder="discussPersonToName" />
				</view>
				<view style="width: 50px;font-size: 11pt;color: #808080;" @click="discussSumbit">发布</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				onlineCourse: {
					onlineCourseDiscussList: []
				},
				discussPersonToName: '说点什么吧',
				discussText: '',
				formData: {},
				onlineCourseParam: undefined,
				starDiscussList: [],
				inputBottom: 0
			}
		},
		computed: {
			selectIsGood() {
				return function(id) {
					let goal = this.onlineCourse.onlineCourseId + "_" + id
					for(let star of this.starDiscussList) {
						if(star == goal) {
							return "../../static/icon/good-color.png"
						}
					}
					return "../../static/icon/good.png"
				}
			},
			showEdit() {
				return function(discussPerson) {
					const studentNum = uni.getStorageSync("studentNum")
					if(studentNum == discussPerson) {
						return true
					}
					return false
				}
			}
		},
		methods: {
			inputFocus(e) {
				console.log(e)
				if(e.detail.height) {
					this.inputBottom = e.detail.height
				}
			},
			inputBlur() {
				this.inputBottom = 0
			},
			selectIsGoodTwo: function() {
				if(this.onlineCourse != null) {
					let goal = this.onlineCourse.onlineCourseId + "_" + this.onlineCourse.id
					for (let star of this.starDiscussList) {
						if (star == goal) {
							return false
						}
					}
					return true
				}
			},
			discussSumbit() {
				if (this.discussText.length == 0) {
					return;
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
						if (res.data.status == 200) {
							this.discussText = ''
							uni.showToast({
								title: "评论成功",
								duration: 2000
							})
							this.getDiscussPersonList()
						} else {
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
					onlineCourseId: this.onlineCourse.onlineCourseId,
					discussPersonName: uni.getStorageSync("studentName"),
					discussPersonPic: uni.getStorageSync("studentAvatarUrl")
				}
				return formdata
			},
			discussPersonThis(discussParent, discussPersonToName, discussPerson) {
				this.discussPersonToName = '回复 @' + discussPersonToName + ':'
				let formdata = this.setDefaultForm()
				formdata['discussParent'] = discussParent
				formdata['discussToPersonId'] = discussPerson
				this.formData = formdata
			},
			replyDiscuss(discussParent,discussPerson, discussToPerson, discussPersonToName) {
				console.log(discussParent)
				this.discussPersonToName = '回复 @' + discussPersonToName + ':'
				let formdata = this.setDefaultForm()
				formdata['discussParent'] = discussParent
				formdata['discussToPersonId'] = discussPerson
				formdata['discussToPersonName'] = discussPersonToName
				formdata['discussToPerson'] = discussToPerson
				this.formData = formdata
			},
			getDiscussPersonList() {
				return new Promise((resolve,reject) => {
					uni.request({
						url: getApp().globalData.requestUrl + 'online-course-discuss/list-star',
						data: {
							onlineCourseId: this.onlineCourseParam.onlineCourseId,
							discussPerson: uni.getStorageSync("studentNum")
						},
						header: {
							"Authorization": "Bearer WX_" + uni.getStorageSync("tt")
						},
						success: res => {
							this.starDiscussList = res.data.result
							uni.request({
								url: getApp().globalData.requestUrl + 'common/list-discussPerson',
								data: this.onlineCourseParam,
								header: {
									"Authorization": "Bearer WX_" + uni.getStorageSync("tt"),
								},
								success: res => {
									if (res.data.status == 200) {
										let course = res.data.result
										this.onlineCourse = course
										this.discussPersonThis(course.id, course.discussPersonName, course.discussPerson)
										resolve()
									} else {
										uni.showToast({
											icon: 'none',
											title: "服务器出现了故障",
											duration: 2000
										})
									}
								}
							})
						}
					})
				})
			},
			setDefaultStarForm(discussParent,id, discussToPerson) {
				const starForm = {
					discussParent: discussParent,
					onlineCourseId: this.onlineCourse.onlineCourseId,
					starPerson: uni.getStorageSync("studentNum"),
					discussCourseId: id,
					discussToPerson: discussToPerson
				}
				return starForm
			},
			isExisStar(id) {
				let goal = this.onlineCourse.onlineCourseId + "_" + id
				for (let index in this.starDiscussList) {
					if (this.starDiscussList[index] == goal) {
						return index
					}
				}
				return -1
			},
			clickFirstGood(discussParent,id,discussToPerson, discuss) {
				let starForm = this.setDefaultStarForm(discussParent,id,discussToPerson)
				uni.request({
					url: getApp().globalData.requestUrl + 'online-course-discuss/click-star',
					method: 'POST',
					data: starForm,
					header: {
						"Authorization": "Bearer WX_" + uni.getStorageSync("tt"),
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if (res.data.status == 200) {
							let indexOf = this.isExisStar(id)
							uni.showToast({
								title: "点赞成功",
								duration: 2000
							})
							if (indexOf != -1) {
								discuss.star = discuss.star - 1
								this.starDiscussList.splice(indexOf, 1)
							} else {
								discuss.star = discuss.star + 1
								this.starDiscussList.push(this.onlineCourse.onlineCourseId + "_" + id)
							}
						} else {
							uni.showToast({
								icon: 'none',
								title: "服务器出现了故障",
								duration: 2000
							})
						}
					}
				})
			},
			clickGood(discussParent,id,discussPerson, discussToPerson, discuss) {
				let starForm = this.setDefaultStarForm(discussParent,id,discussToPerson)
				starForm['discussPerson'] = discussPerson
				uni.request({
					url: getApp().globalData.requestUrl + 'online-course-discuss/click-star',
					method: 'POST',
					data: starForm,
					header: {
						"Authorization": "Bearer WX_" + uni.getStorageSync("tt"),
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if (res.data.status == 200) {
							let indexOf = this.isExisStar(id)
							if (indexOf != -1) {
								discuss.star = discuss.star - 1
								this.starDiscussList.splice(indexOf, 1)
							} else {
								discuss.star = discuss.star + 1
								this.starDiscussList.push(this.onlineCourse.onlineCourseId + "_" + id)
							}
						} else {
							console.log('某种错误')
						}
					}
				})
			},
			async init() {
				await this.getDiscussPersonList()
			}
		},
		onLoad: function(option) {
			const onlineCourseParam = JSON.parse(decodeURIComponent(option.onlineCourseParam))
			this.onlineCourseParam = onlineCourseParam
			this.init()
		}
	}
</script>

<style>
	.discuss {
		display: flex;
		flex-direction: column;
		padding: 15px 8px;
	}
	
	.discuss-context-left {
		display: flex;
		align-items: center;
	}
	
	.discuss-context-right {
		font-size: 9pt;
		color: #808080;
	}

	.discussToGray {
		background-color: #e5e5e5;
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
		width: 100%;
		height: 45px;
		z-index: 2;
		color: #808080;
		display: flex;
		align-items: center;
		justify-content: space-around;
		background-color: white;
	}

	.discuss-button-in {
		width: 100%;
		background-color: white;
		height: 55px;
		display: flex;
		z-index: 10;
		align-items: center;
		border-top: 1px solid #F7F7F7;
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
</style>
