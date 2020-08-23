<template>
    <view>
		<view style="padding: 20upx;" class="layout1">
			<view style="width: 510upx;height: 70upx;line-height: 70upx;font-size: 40upx;color: #808080;font-weight: bold;">教学考评问卷</view>
			<view class="view1" @click="back1()">提交</view>
		</view>
		<uni-card isShadow="true"  v-for="(item,index) in listarr1" :key="index">
			<view>
				<view class="layout1">
					<view style="color: #808080;">我的课程:</view>
					<view style="margin-left: 10upx;">{{item.courseNmae}}</view>
				</view>
				<view class="layout1" style="margin-top: 20upx;">
					<view style="color: #808080;">任课老师:</view>
					<view style="margin-left: 10upx;">{{item.teacherName}}</view>
				</view>
				<view style="margin-top: 20upx;padding-bottom: 20upx;border-bottom: 1upx solid #007AFF;">
					<text>提问一:上课是否认真负责，不迟到，不早退</text>
					<checkbox-group style="margin-top: 10upx;">
					    <label>
					        <checkbox value="cb" checked="true"/>满意
					    </label>
					    <label style="margin-left: 50upx;">
					        <checkbox value="cb" />一般
					    </label>
						<label style="margin-left: 50upx;">
						    <checkbox value="cb" />不满意
						</label>
					</checkbox-group>
				</view>
				<view style="margin-top: 40upx;padding-bottom: 20upx;border-bottom: 1upx solid #007AFF;">
					<text>提问二:课程内容是否生动易学，有条有理</text>
					<checkbox-group style="margin-top: 10upx;">
					    <label>
					        <checkbox value="cb" checked="true"/>满意
					    </label>
					    <label style="margin-left: 50upx;">
					        <checkbox value="cb" />一般
					    </label>
						<label style="margin-left: 50upx;">
						    <checkbox value="cb" />不满意
						</label>
					</checkbox-group>
				</view>
				<view style="margin-top: 40upx;padding-bottom: 20upx;border-bottom: 1upx solid #007AFF;">
					<text>提问三:课堂气氛是否活跃，有良好的课堂互动</text>
					<checkbox-group style="margin-top: 10upx;">
					    <label>
					        <checkbox value="cb" checked="true"/>满意
					    </label>
					    <label style="margin-left: 50upx;">
					        <checkbox value="cb" />一般
					    </label>
						<label style="margin-left: 50upx;">
						    <checkbox value="cb" />不满意
						</label>
					</checkbox-group>
				</view>
			</view>
		</uni-card>
		<!-- 提示提交成功 -->
		<uni-popup ref="tijiao" type="center">
			<view class="view4">提交成功~</view>
		</uni-popup>
	</view>
</template>

<script>
export default {
        data() {
            return {
				listarr1:[
					{courseNmae:"javaEE程序设计",teacherName:"唐老师"},
					{courseNmae:"PHP程序设计",teacherName:"李老师"},
					{courseNmae:"就业指导2",teacherName:"白老师"},
					{courseNmae:"计算机导论",teacherName:"张老师"},
				]
            }
        },
        methods: {
			//返回上一个页面
			back1(){
				this.$refs.tijiao.open()
				setTimeout(function(){
					uni.navigateBack()
				},2000);
			},
        }
    }
</script>

<style>
	view{
		font-size: 30upx;
	}
	.layout1{
		display: flex;
		flex-direction: row;
	}
	.view1{
		display: flex;
		width: 200upx;
		height: 70upx;
		border-radius: 10upx;
		line-height: 70upx;
		font-size: 30upx;
		justify-content: center;
		background-color: #ffaa00;
		color: #FFFFFF;
	}
	.view4{
		background-color: #333333;
		color: #FFFFFF;
		height: 60upx;
		padding-left: 20upx;
		padding-right: 20upx;
		font-size: 30upx;
		display: flex;
		justify-content: center;
		line-height: 60upx;
		border-radius: 10upx;
	}
</style>