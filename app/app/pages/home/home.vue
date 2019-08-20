<template>
	<view>
		<view class="mask" v-show="mask1Hidden" @click="mask1Cancel">
			<nav_choose @onOverallTag="onOverallTags" @onFilter="onFilters" :sortSelected="sortSelecteds"></nav_choose>
			<view class="overall-sort-list">
				<block v-for="(item,index) in sortList" :key="index">
					<view class="overall-sort" :data-index="index" @click="sortSelect">{{item.sort}}</view>
				</block>
			</view>
		</view>

		<view class="mask" v-show="mask2Hidden" @click="mask2Cancel">
			<nav_choose @onOverallTag="onOverallTags" @onFilter="onFilters" :sortSelected="sortSelecteds"></nav_choose>
			<scroll-view class="filterList" scroll-y="true">
				<view class="filterList-characteristic-title">商家特色</view>
				<view class="filterList-characteristic-items">
					<block v-for="(item,index) in characteristicList" :key="index">
						<view :class="characteristicSelected[index]==true?'characteristic-selected':''" class="filterList-characteristic-item"
						 @click.stop="characteristicSelecteds" :data-index="index">{{item.text}}</view>
					</block>
				</view>
				<view class="filterList-discount-title">优惠活动(单选)</view>
				<view class="filterList-discount-items">
					<block v-for="(item,index) in discountList" :key="index">
						<view :class="discountSelected==index?'discount-selected':''" class="filterList-discount-item" @click.stop="discountSelecteds"
						 :data-index="index">
							<text class="filterList-discount-item-icon" :style="{background:item.iconColor}">{{item.icon}}</text>
							{{item.text}}</view>
					</block>
				</view>
			</scroll-view>
			<view class="filterList-footer">
				<view class="filterList-footer-delect" @click.stop="clearSelectedNumb">清除筛选</view>
				<view class="filterList-footer-finish" @click="finish">完成
					<view class="filterList-footer-finish-number" v-show="selectedNumb!=0">{{selectedNumb}}
					</view>
				</view>
			</view>
		</view>


		<scroll-view bindscrolltolower="lower" class="scroll-restaurants-list" scroll-y="true" style="height:100%">
			<view class="heard">
				<view class="heard-location" @click="getLocation()">
					<image src="/static/home_pic/location.png" class="heard-location-icon" />
					<text class="heard-location-text">
						<!-- {{location}} -->{{Adrr}}</text>
				</view>
				<navigator url="../search/search" hover-class="none">
					<view class="heard-search" hover-class="none">
						<image src="/static/home_pic/search.png" class="heard-search-icon" />
						{{dish_name}}
					</view>
				</navigator>
			</view>
			<swiper class="categoryList" indicator-dots="true" indicator-color="rgba(228,228,228,1)" indicator-active-color="#FECA49">
				<swiper-item>
					<block v-for="(item, i) in categoryList" :key="i">
						<view class="category-info">
							<image :src="item.src" class="category-image"></image>
							<view class="category-text">{{item.name}}</view>
						</view>
					</block>
				</swiper-item>
				<swiper-item>
					<block v-for="(item, i) in categoryList" :key="i">
						<view class="category-info">
							<navigator url="" hover-class="none">
								<image :src="item.src" class="category-image"></image>
								<view class="category-text">{{item.name}}</view>
							</navigator>
						</view>
					</block>
				</swiper-item>
			</swiper>
			<view class="header-title">附近商家</view>
			<nav_choose @onOverallTag="onOverallTags" @onFilter="onFilters" :sortSelected="sortSelecteds"></nav_choose>

			<view class="restaurantsList">
				<block v-for="(item,index) in restaurant" :key="index">
					<navigator url="/pages/shoping-car/shopingCar">
						<view class="restaurants-list">
							<view class="restaurants-info-image">
								<image :src="item.src" class="restaurants-image" />
							</view>
							<view class="restaurants-info">
								<view class="restaurants-info-name">{{item.name}}</view>
								<view class="restaurants-info-rating">
									<view class="restaurants-info-rating-stars">★ ★ ★ ★</view>
									<view class="restaurants-info-rating-sales">月售 {{item.sales}}单</view>
								</view>
								<view class="restaurants-info-price">
									起送 {{item.initial_price}}¥ | 配送 {{item.distribution_price}}¥
								</view>
							</view>
							<view class="restaurants-distribution">
								<view class="restaurants-distribution-distance">
									{{item.distance}}
								</view>
								<view class="restaurants-distribution-time">
									{{item.time}} 分钟
								</view>
							</view>
						</view>
					</navigator>
				</block>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	import Vue from 'vue';
	import nav_choose from '@/components/nav_choose.vue'
	export default {
		components: {
			nav_choose
		},
		data() {
			return {
				Adrr:'请选择地址',
				dish_name:'点击搜索',
				characteristicList: [{
					text: "免配送费"
				}, {
					text: "0元起送"
				}, {
					text: "新商家"
				}, {
					text: "品牌商家"
				}, {
					text: "跨天预定"
				}],
				sortList: [{
					sort: "综合排序",
					image: "",
				}, {
					sort: "速度最快",
					image: "",
				}, {
					sort: "评分最高",
					image: "",
				}, {
					sort: "起送价最低",
					image: "",
				}, {
					sort: "配送费最低",
					image: "",
				}],
				discountList: [{
					icon: "减",
					iconColor: "#FF635B",
					text: "满减优惠"
				}, {
					icon: "领",
					iconColor: "#FF7298",
					text: "进店领券"
				}, {
					icon: "返",
					iconColor: "#FB4343",
					text: "满返代金券"
				}, {
					icon: "折",
					iconColor: "#C183E2",
					text: "折扣商品"
				}, {
					icon: "订",
					iconColor: "#6FDF64",
					text: "提前下单优惠"
				}, {
					icon: "赠",
					iconColor: "#FDC41E",
					text: "满赠活动"
				}, {
					icon: "免",
					iconColor: "#43B697",
					text: "满免配送"
				}],
				categoryList: [{
					"src": '../../static/home_pic/1.png',
					"name": '美食',
					"id": 1
				}, {
					"src": '../../static/home_pic/2.png',
					"name": '甜点饮品',
					"id": 2
				}, {
					"src": '../../static/home_pic/3.png',
					"name": '美团超市',
					"id": 3
				}, {
					"src": '../../static/home_pic/4.png',
					"name": '正餐精选',
					"id": 4
				}, {
					"src": '../../static/home_pic/5.png',
					"name": '生疏果鲜',
					"id": 5
				}, {
					"src": '../../static/home_pic/6.png',
					"name": '全部商城',
					"id": 6
				}, {
					"src": '../../static/home_pic/7.png',
					"name": '免费配送',
					"id": 7
				}, {
					"src": '../../static/home_pic/8.png',
					"name": '新商家',
					"id": 8
				}],
				selected: 0,
				mask1Hidden: false,
				mask2Hidden: false,
				animationData: "",
				location: "",
				characteristicSelected: [false, false, false, false, false, false, false],
				discountSelected: null,
				selectedNumb: 0,
				restaurant: [],
				sortSelecteds: "综合排序"
			}
		},
		methods: {
			mask1Cancel: function() {
				var that = this;
				that.mask1Hidden = false;
			},
			mask2Cancel: function() {
				var that = this;
				that.mask2Hidden = false;
			},
			sortSelect: function(e) {
				var that = this;
				that.sortSelecteds = that.sortList[e.currentTarget.dataset.index].sort
			},
			sortSelected: function(e) {
				var that = this;
				uni.request({
					url: "https://www.easy-mock.com/mock/5d3726935317be31dd341b39/takeOut/merchant_list",
					method: "GET",
					success: function(res) {
						for (let x of res.data.data.restaurant) {
							that.restaurant.push(x);
						}
					}
				});
			},
			onFilters: function() {
				var that = this;
				that.mask2Hidden = true
			},
			onOverallTags: function() {
				var that = this;
				that.mask1Hidden = true
			},
			characteristicSelecteds: function(e) {
				var that = this;
				var info = that.characteristicSelected;
				info[e.currentTarget.dataset.index] = !info[e.currentTarget.dataset.index];
				that.selectedNumb = that.selectedNumb + (info[e.currentTarget.dataset.index] ? 1 : -1);
				that.characteristicSelected = info;
			},
			discountSelecteds: function(e) {
				var that = this;
				if (that.discountSelected != e.currentTarget.dataset.index) {
					that.selectedNumb = that.selectedNumb + (that.discountSelected == null ? 1 : 0);
					that.discountSelected = e.currentTarget.dataset.index;
				} else {
					that.selectedNumb = that.selectedNumb - 1;
					that.discountSelected = null;
				}
			},
			finish: function() {
				var that = this;
				var y = 0;
				wx.request({
					url: "https://www.easy-mock.com/mock/5d3726935317be31dd341b39/takeOut/merchantList1",
					method: "GET",
					success: function(res) {
						for (let x of res.data.data.restaurant) {
							Vue.set(that.restaurant, y, x);
							y++;
						}
					}
				});
			},
			clearSelectedNumb: function() {
				var that = this;
				that.discountSelected = null;
				that.selectedNumb = 0;
				that.characteristicSelected = [false];
			},
			getLocation: function() {
				var that=this;
				uni.chooseLocation({
				    success: function (res) {			        
						that.Adrr=res.name;				      
				    }
				});
			}
		},
		onReady() {
			this.sortSelected();
		},
		onPullDownRefresh() {
			console.log("开始刷新");
			uni.stopPullDownRefresh(); //放在数据请求成功的函数里面
		},
		onShow() {
			var that = this;
			var globalDish_name=getApp().globalData;
			that.dish_name=globalDish_name.dish_name;
		}
	}
</script>

<style>
	.heard-location-text {
		position: absolute;
		overflow: hidden;
	}

	.overall-sort-list,
	.filterList,
	.filterList-footer {
		background: white;
	}

	.overall-sort-list {
		flex-direction: column;
		border-top: 1px solid rgba(0, 0, 0, 0);
		display: flex;
	}

	.overall-sort {
		font-size: 25rpx;
		height: 70rpx;
		line-height: 70rpx;
		margin-left: 30rpx;
	}

	.filterList {
		height: 500rpx;
		font-size: 27rpx;
		overflow: hidden;
	}

	.filterList-characteristic-title,
	.filterList-discount-title {
		position: relative;
		height: 50rpx;
		line-height: 50rpx;
		background: #FBFBFB;
		font-weight: 400;
		padding-left: 30rpx;
	}

	.filterList-characteristic-title::before,
	.filterList-discount-title::before {
		content: "";
		position: absolute;
		left: 0;
		top: 15rpx;
		display: inline-block;
		width: 5rpx;
		height: 20rpx;
		background: #FED161;
	}

	.filterList-characteristic-items {
		height: 200rpx;
		display: flex;
		flex-wrap: wrap;
		justify-content: flex-start;
		align-content: space-around;
	}

	.filterList-characteristic-item {
		text-align: center;
		height: 70rpx;
		width: 200rpx;
		margin: 0 20rpx;
		border-radius: 70rpx;
		border: 2rpx solid #ECECEC;
		line-height: 70rpx;
	}

	.filterList-discount-item-icon {
		padding: 0 4rpx;
		color: white;
	}

	.filterList-discount-item {
		margin-left: 30rpx;
		height: 80rpx;
		line-height: 80rpx;
		border-bottom: 1rpx solid #ECECEC;
	}

	.filterList-footer {
		height: 90rpx;
		display: flex;
	}

	.filterList-footer-delect {
		flex: 1;
		font-size: 35rpx;
		line-height: 90rpx;
		justify-content: center;
		align-items: center;
		text-align: center;
	}

	.filterList-footer-finish {
		display: flex;
		flex: 1;
		justify-content: center;
		align-items: center;
		text-align: center;
		line-height: 90rpx;
		font-size: 35rpx;
		width: 210rpx;
		background: #FFD161;
		position: relative;
	}

	.filterList-footer-finish-number {
		height: 30rpx;
		width: 30rpx;
		line-height: 31rpx;
		font-size: 25rpx;
		margin-left: 15rpx;
		color: #FFD161;
		border-radius: 50%;
		background: black;
	}

	.characteristic-selected {
		background: #FFFBF1;
		border-color: #FFE9B7;
		color: #FFE6B4;
	}

	.discount-selected {
		color: #FFE6B4;
	}

	.heard {
		justify-content: space-around;
		display: flex;
		width: 100%;
		height: 80rpx;
		background: #FFC640;
	}

	.heard-location-icon {
		position: absolute;
		top: 12rpx;
		height: 25rpx;
		width: 25rpx;
	}

	.heard-location-text {
		left: 47rpx;
		width: 190rpx;
		white-space: nowrap;
		text-overflow: ellipsis;
	}

	.heard-location {
		position: relative;
		font-size: 25rpx;
		padding: 5rpx 15rpx;
		margin: 10rpx;
		width: 250rpx;
		height: 40rpx;
		background: #B38B2D;
		color: white;
		border-radius: 20rpx;
	}

	.heard-location:after {
		content: '';
		position: absolute;
		top: 20rpx;
		left: 250rpx;
		width: 8rpx;
		height: 8rpx;
		border: 3rpx solid #fff;
		border-bottom: none;
		border-left: none;
		transform: rotate(45deg);
	}

	.heard-search {
		font-size: 25rpx;
		padding: 5rpx 15rpx;
		margin: 10rpx;
		height: 40rpx;
		width: 400rpx;
		background: white;
		border-radius: 20rpx;
		line-height: 40rpx;
	}

	.heard-search-icon {
		height: 21rpx;
		width: 21rpx;
	}

	.categoryList {
		width: 100%;
		height: 350rpx;
		border-bottom: 20rpx solid #f4f4f4;
	}

	.category-info {
		display: inline-block;
		text-align: center;
		position: relative;
		margin-top: 20rpx;
		height: 150rpx;
		width: 25%;
	}

	.category-image {
		width: 95rpx;
		height: 95rpx;
	}

	.category-text {
		font-size: 25rpx;
		width: 100%;
		line-height: 30rpx
	}

	.header-title {
		text-align: center;
		width: 100%;
		height: 80rpx;
		line-height: 80rpx;
		font-size: 31rpx;
		font-weight: 600;
	}

	.sort-list {
		justify-content: space-around;
		display: flex;
		background: white;
		width: 100%;
		height: 80rpx;
		font-size: 27rpx;
		color: #6A6A6A;
		align-content: center;
		border: 1rpx solid #ECECEC;
	}

	.sort {
		text-align: center;
		margin: auto 0;
		height: 30rpx;
		line-height: 30rpx;
		border-right: 1rpx solid #ECECEC;
		flex: 1;
	}

	.sort-active {
		color: black;
		font-weight: 800;
	}

	.restaurants-list {
		font-size: 25rpx;
		display: flex;
		width: 100%;
		padding: 10rpx 0;
		border-bottom: 1rpx solid #ECECEC;
		color: #6D6D6D;
		justify-content: space-between;
	}

	.restaurants-info-name {
		color: black;
		font-size: 30rpx;
		margin-bottom: 10rpx;
	}

	.restaurants-image {
		width: 200rpx;
		height: 125rpx;
		margin: 20rpx;
	}

	.restaurants-info {
		margin: 15rpx;
		flex: 1;
	}

	.restaurants-distribution {
		display: flex;
		width: 100rpx;
		margin-right: 20rpx;
	}

	.restaurants-info-rating {
		display: flex;
		width: 300rpx;
		height: 50rpx;
		line-height: 50rpx;
	}

	.restaurants-info-rating-stars {
		color: #FFD161;
		width: 130rpx;
		height: 50rpx;
	}

	.restaurants-info-rating-sales {
		width: 150rpx;
		height: 50rpx;
	}

	.restaurants-distribution {
		flex-direction: column;
		text-align: right;
		justify-content: center;
	}

	.mask {
		width: 100%;
		height: 100%;
		position: fixed;
		z-index: 999;
		background-color: rgba(15, 15, 26, 0.7);
	}
</style>
