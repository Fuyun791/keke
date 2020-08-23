<template>
	<view>
		<view @click="switchSearch">
			<!-- 搜索框 -->
			<uni-search-bar :radius="100" placeholder="请输入课程名称" @confirm="searchSumbit" @input="searchInput"></uni-search-bar>
		</view>
		<view style="height: 7px;width: 100%;background-color: #F7F7F7;"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return{
				
			}
		},
		methods: {
			switchSearch() {
				console.log(8)
			},
			searchSumbit(e) {
				console.log(e)
			},
			searchInput(e) {
				console.log(e)
			}
		}
	}
</script>

<style>
</style>
