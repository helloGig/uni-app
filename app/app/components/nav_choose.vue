<template>
	<view class="sort-list">
		<view class="sort" @click="onOverallTag">{{sortSelected}}
			<image src="../static/home_pic/down.png" style="width:20rpx;height:20rpx; padding-left: 2px;"></image>
		</view>
		<view class="sort" @click="onTapTag" data-id="1" :class="isActive==1?'sort-active':''">
			销量最高
		</view>
		<view class="sort" @click="onTapTag" data-id="2" :class="isActive==2?'sort-active':''">
			距离最近
		</view>
		<view class="sort" @click="onFilter">筛选
			<image src="../static/home_pic/screen.png" style="width:22rpx;height:22rpx;padding-left: 2px;"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isActive: 0,
				flag: false
			}
		},
		props: {
			sortSelected: {
				required: false,
				type: String,
				default: ""
			}
		},
		methods: {
			onTapTag: function(e) {
				var that = this;
				that.isActive = e.target.dataset.id;
				console.log(that.isActive);
			},
			onOverallTag: function() {
				this.$emit('onOverallTag');
			},
			onFilter: function() {
				this.$emit('onFilter');
			}
		}
	}
</script>

<style>

</style>
