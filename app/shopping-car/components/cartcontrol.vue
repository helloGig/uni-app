<template>
	<view style="display: flex;flex-direction: row;height: 27px;">
		<view class="food-control">
			<view class="cont" style="margin-top: 2px;" @click="decreaseCart(food)" v-show="food.count>0">
				<image src="/static/des.png" mode="" style="width: 20px;height: 20px;"></image>
			</view>
			<text style="padding: 0 4px;" v-show="food.count>0">{{food.count}}</text>
		</view>
		<view style="flex: 1;margin-top: 2px; " class="cont" @click="addCart(food)">
			<image src="/static/add2.png" style="width: 20px;height: 20px;color: #CCCCCC;" mode=""></image>
		</view>
	</view>
</template>

<script>
	// import Vue from 'vue'
	export default {
		props: {
			food: {
				type: Object,
			}
		},
		data() {
			return {
			};
		},
		methods: {
			addCart: function(item) {
				this.$emit('add',item)
			},
			decreaseCart(item) {
				this.$emit('dec',item)
			}
		}
	}
</script>

<style scoped>
	.food-control {
		display: flex;
		flex: 1;
		justify-content: space-around;
	}

	.cont {
		width: 22px;
		height: 22px;
		box-sizing: border-box;
		border-radius: 50%;
		text-align: center;
	}
</style>
