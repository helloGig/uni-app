<template>
	<view>
		<view class="anglesList">
			<navigator url="" class="anglesNavigator" hover-class="none">
				<view class="imgBox">
					<image src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566276830018&di=42605cb0234691bfa383c17e1263bc62&imgtype=0&src=http%3A%2F%2Fhiphotos.baidu.com%2Fnuomi%2Fpic%2Fitem%2F4afbfbedab64034f978d0ac7a6c379310b551dab.jpg"></image>
				</view>
				<view class="textBox">
					<text class="name">配送约33分钟</text>
					<text class="companyName">公告: 亲爱的顾客您好,我们绝对不卖过夜的食品...</text>
					<view class="tag">
						<view class="">新人优惠</view>
						<view class="">减18</view>
						<view class="">25减15</view>
						<view class="">津贴1元</view>
					</view>
				</view>
			</navigator>
			<navigator url="" class="btnFollowed">关注</navigator>
		</view>
		<view class="" style="height: 100%; width: 100%;background: #DD524D;">
			<Tabs :TabList="TabList" :currentTab="current" @tabs="tabsChange">
				<!-- 写页面 -->
				<TabPane>
					<view class="content">
						<scroll-view class="menu-wrapper" scroll-y :style="'height:'+height+'px'">
							<view ref="menuWrapper">
								<!--  :class="{'current': currentIndex == index}" @click="selectMenu(index,$event)" -->
								<view style="position: relative;" v-for="(item,index) in goods" :key="index" ref="menuList" @click="select(index)"
								 :class="{'current': currentIndex == index}">

									<view class="menu-item">{{item.name}}</view>
									<text class="allcount" v-if="getAllCount>=item.count&&item.count>0">{{item.count}}</text>
									<!-- <text class="allcount">1</text> -->
								</view>
							</view>
						</scroll-view>
						<!--  @scroll="scroll" -->
						<scroll-view class="foods-wrapper" scroll-y :style="'height:'+height+'px'" :scroll-top="foodSTop">
							<view ref="foodsWrapper">
								<view ref="foodList" class="foods" v-for="(item, i) in goods" :key="i">

									<view class="food-title" style="background: #f3f5f7">
										{{item.name}}
									</view>
									<view class="food" v-for="(food, index) in item.foods" :key="index">
										<image :src="food.img" mode="" style="width: 75px;height: 75px;margin-top: 6px;"></image>
										<view class="food-info">
											<text style="font-size: 15px;margin-top: 2px;">{{food.name}}</text>
											<text style="font-size: 13px;margin: 2px 0;">{{food.description}}</text>
											<text style="font-size: 13px;margin: 2px 0 4px;">月售{{food.sellCount}}</text>

											<!-- 加减 -->
											<view class="food-btm">
												<text class="food-price">￥{{food.price}}</text>
												<cartcontrol :food="food" @add="addCart" @dec="decreaseCart"></cartcontrol>

											</view>
										</view>
									</view>
								</view>
							</view>
						</scroll-view>
						<shopcart :goods="goods" @add="addCart" @dec="decreaseCart" @delAll="delAll"></shopcart>
					</view>
				</TabPane>
				<TabPane>
					<view class="commentPage-header">
						<view class="commentPage-score">
							<view class="overall">综合评分</view>
							4.3
							<view class="rate">商家综合好评率为76%</view>
						</view>
						<view class="commentPage-rating">
							<view class="restaurant">商家评分
								<view class="stars" style="color: #C6E746;">★★★★★</view>
							</view>
							<view class="distribute">配送评分
								<view class="stars" style="color: #C6E746;">★★★★★</view>
							</view>
						</view>
					</view>
					<view class="commentPage-category">
						<block v-for="(item,index) in categoryList" :key="index">
							<view class="category-item" :class="active_flag==index?'category-item-active':''" @click.stop="category_active"
							 :data-id="index">{{item.name}}({{item.Nub}})</view>
						</block>
					</view>

					<view class="uni-padding-wrap">
						<!-- 评论区 start -->
						<view class="uni-comment">
							<block v-for="(item,index) in critic" :key="index">
								<view class="uni-comment-list">
									<view class="uni-comment-face">
										<image :src="item.handPic" mode="widthFix"></image>
									</view>
									<view class="uni-comment-body">
										<view class="uni-comment-top">
											<text>{{item.name}}</text>
										</view>
										<view class="uni-comment-content">{{item.content}}</view>
									</view>
								</view>
							</block>
						</view>
					</view>

				</TabPane>
				<TabPane>
					<view class="center">
						<view class="center-list">
							<view class="center-list-item" hover-class="logo-hover">
								<text class="list-icon">&#xe614;</text>
								<text class="list-text">江北区</text>
								<text class="navigat-arrow">&#xe65e;</text>
							</view>
						</view>
						<view class="center-list">
							<view class="center-list-item" hover-class="logo-hover">
								<text class="list-icon">&#xe614;</text>
								<text class="list-text">查看食品</text>
								<text class="navigat-arrow">&#xe65e;</text>
							</view>
						</view>
						<view class="center-list">
							<view class="center-list-item" hover-class="logo-hover">
								<text class="list-icon">&#xe614;</text>
								<text class="list-text">配送服务:由美团配送</text>
								<text class="navigat-arrow">&#xe65e;</text>
							</view>
						</view>			
					</view>
						
				</TabPane>
			</Tabs>
		</view>

	</view>
</template>

<script>
	import shopcart from '@/components/shopcart.vue'
	import cartcontrol from '@/components/cartcontrol.vue'
	import Tabs from '@/components/tabs.vue'
	import TabPane from '@/components/tabPane.vue'

	import Vue from 'vue'
	// import BScroll from 'better-scroll'

	export default {
		data() {
			return {
				title: 'Hello',
				current: 0,
				critic: [{
						name: "卢本伟",
						handPic: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566220009225&di=36430f73aac8c38f383030f42fa1f230&imgtype=0&src=http%3A%2F%2Fimg.tukexw.com%2Fimg%2Fb8d88b41eda68424.jpg",
						content: "我卢本伟没有开挂,开挂死全家。粉丝是我家人🌚"
					},
					{
						name: "PDD",
						handPic: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566220861198&di=581c5fc434e31122673f8b6bcb13ea7c&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fq_70%2Cc_zoom%2Cw_640%2Fimages%2F20190106%2F056737bcfb67410f94818fd353a9df72.jpeg",
						content: "老板给我来两斤皮皮虾！"
					},
					{
						name: "菜徐坤",
						handPic: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566220973155&di=d66c8e0dea6a13c9fdaf418cefb13e69&imgtype=0&src=http%3A%2F%2Fww2.sinaimg.cn%2Fbmiddle%2F006AvlX9ly1g21bbnq0jcj309t09cmxo.jpg",
						content: "大家好我是吃饭两年半的个人练习生鸡你太美🏀"
					},
					{
						name: "热血青年",
						handPic: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566221039398&di=3713e86d9f0766812fc4e75396d370e7&imgtype=0&src=http%3A%2F%2Fimg.mp.sohu.com%2Fq_70%2Cc_zoom%2Cw_640%2Fupload%2F20170807%2F874e57fabc084dd395da59aad336c070_th.jpg",
						content: "数风流人物,还看今朝"
					},
					{
						name: "面筋哥",
						handPic: "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2751525850,4266787092&fm=26&gp=0.jpg",
						content: "这家店的东西比我的面筋好吃一万倍!"
					}, {
						name: "波澜哥",
						handPic: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566221155162&di=692cf183cbd299c062b4023040a1ee64&imgtype=0&src=http%3A%2F%2Fi2.hdslb.com%2Fbfs%2Farchive%2F10ac40937a4f605ed197125b5d81dbcd6c6e7123.jpg_320x200.jpg",
						content: "你嫌弃破烂，抛弃了我"
					}
				],
				categoryList: [{
					name: '全部',
					Nub: '60'
				}, {
					name: '有图评价',
					Nub: '20'
				}, {
					name: '好评',
					Nub: '30'
				}, {
					name: '差评',
					Nub: '1'
				}, {
					name: '味道赞',
					Nub: '6'
				}, {
					name: '价格实惠',
					Nub: '2'
				}, {
					name: '服务好',
					Nub: '1'
				}],
				TabList: [{
						title: '菜单'
					},
					{
						title: '评价'
					},
					{
						title: '商家'
					}
				],
				goods: [{
						"name": "热销",
						"foods": [{

								"name": "南瓜粥",
								"price": 9.22,
								"oldPrice": "",
								"description": "食材：大米，南瓜",
								"sellCount": 229,
								"img": '../../static/shopingCar/nanguaz.jpg'

							}, {
								"name": "小米粥",
								"price": 9.8,
								"oldPrice": "",
								"description": "食材：小米",
								"sellCount": 239,
								"img": '../../static/shopingCar/xiaomi.jpg'

							},
							{
								"name": "油条",
								"price": 1.88,
								"oldPrice": "",
								"description": "食材：油条",
								"sellCount": 229,
								"img": '../../static/shopingCar/youtiao.jpg'

							}
						]
					},
					{
						"name": "折扣",
						"foods": [{
							"name": "油条1只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "套餐",
						"foods": [{
							"name": "油条2只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥2",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "套餐2",
						"foods": [{
							"name": "油条3只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥3",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "热销2",
						"foods": [{
							"name": "南瓜粥1",
							"price": 9.22,
							"oldPrice": "",
							"description": "食材：大米，南瓜",
							"sellCount": 229,
							"img": '../../static/shopingCar/nanguaz.jpg'

						}, {
							"name": "小米粥1",
							"price": 9.8,
							"oldPrice": "",
							"description": "食材：小米",
							"sellCount": 239,
							"img": '../../static/shopingCar/xiaomi.jpg'

						}]
					},
					{
						"name": "折扣2",
						"foods": [{
							"name": "油条4只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥4",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "套餐3",
						"foods": [{
							"name": "油条5只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥5",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "套餐4",
						"foods": [{
							"name": "油条6只",
							"price": 1.81,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥6",
							"price": 9.91,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "热销3",
						"foods": [{
							"name": "南瓜粥3",
							"price": 9.21,
							"oldPrice": "",
							"description": "食材：大米，南瓜",
							"sellCount": 229,
							"img": '../../static/shopingCar/nanguaz.jpg'

						}, {
							"name": "小米粥3",
							"price": 9.78,
							"oldPrice": "",
							"description": "食材：小米",
							"sellCount": 239,
							"img": '../../static/shopingCar/xiaomi.jpg'

						}]
					},
					{
						"name": "折扣3",
						"foods": [{
							"name": "油条7只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥7",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "套餐6",
						"foods": [{
							"name": "油条8只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥8",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "套餐7",
						"foods": [{
							"name": "油条9只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥9",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},
					{
						"name": "热销4",
						"foods": [{
							"name": "南瓜粥4",
							"price": 9.22,
							"oldPrice": "",
							"description": "食材：大米，南瓜",
							"sellCount": 229,
							"img": '../../static/shopingCar/nanguaz.jpg'

						}, {
							"name": "小米粥4",
							"price": 9.8,
							"oldPrice": "",
							"description": "食材：小米",
							"sellCount": 239,
							"img": '../../static/shopingCar/xiaomi.jpg'

						}]
					},
					{
						"name": "折扣4",
						"foods": [{
							"name": "油条10只",
							"price": 1.88,
							"oldPrice": "",
							"description": "食材：油条",
							"sellCount": 229,
							"img": '../../static/shopingCar/youtiao.jpg'

						}, {
							"name": "艇仔粥10",
							"price": 9.9,
							"oldPrice": "",
							"description": "食材：瘦肉，干贝，花生等",
							"sellCount": 239,
							"img": '../../static/shopingCar/tingzaiz.jpg'

						}],

					},

				],
				height: 0,
				foodSTop: 0,
				currentIndex: 0,
				active_flag: 0
			}

		},
		components: {
			shopcart,
			cartcontrol,
			Tabs,
			TabPane
		},
		onLoad() {
			this.height = Number(uni.getSystemInfoSync().windowHeight) - 55;
			// console.log(uni.getSystemInfoSync().windowHeight)
		},

		computed: {
			getList() {
				let result = [];
				this.goods.forEach((good) => {

					good.foods.forEach((food) => {

						if (food.count) {
							result.push(food)
						}
					})
				})
				// console.log('result', result);
				return result

			},
			// 获得购物车所有商品数量
			getAllCount: function(item) {
				// console.log('item', item)
				let result = [];
				let count = 0;

				for (let i = 0; i < this.goods.length; i++) {
					count = 0;
					this.goods[i].foods.forEach((food) => {

						// console.log('food',food);
						if (food.count >= 0) {
							count += food.count
							Vue.set(this.goods[i], 'count', count)
						}
					})
					// console.log('result', count);
					result.push(count)
				}

				result.sort(function(a, b) {
					return a - b;
				})
				count = result[result.length - 1]
				// console.log('result', count);
				return count;
			},

		},
		methods: {
			category_active: function(e) {
				var that = this;
				that.active_flag = e.currentTarget.dataset.id;
				console.log(e.currentTarget.dataset.id);
			},
			// 点击侧边栏
			select(index) {
				this.currentIndex = index;
				this.setScrollH(index);

			},
			tabsChange(index) {
				this.current = index
			},
			// 设置点击侧边栏右边滚动的高度
			setScrollH: function(index) {
				var that = this;
				let height = 0;
				var query = uni.createSelectorQuery();
				// console.log('query',query);
				let foods = query.selectAll('.foods');
				// console.log('foods', foods);

				this.$nextTick(function() {
					foods.fields({
						size: true
					}, data => {
						if (index == 0) {
							that.foodSTop = 0;
						}
						for (let i = 0; i < index; i++) {

							height += parseInt(data[i].height);
							// console.log('fh', foods);
							that.foodSTop = height;
							// console.log(that.foodSTop)
						}

					}).exec();
				})

			},

			addCart: function(item) {
				// console.log('ev', JSON.stringify(item))
				if (item.count >= 0) {
					item.count++
					this.goods.forEach((good) => {
						good.foods.forEach((food) => {
							if (item.name == food.name)
								food.count = item.count
						})
					})
					// console.log('c++', JSON.stringify(item))

				} else {
					console.log('add')
					this.goods.forEach((good) => {
						good.foods.forEach((food) => {
							if (item.name == food.name)
								Vue.set(food, 'count', 1)
							// food.count = 1
							// console.log('add-shop', JSON.stringify(food))
						})
					})
				}

			},
			decreaseCart(item) {
				if (item.count) {
					item.count--
					this.goods.forEach((good) => {
						good.foods.forEach((food) => {
							if (item.name == food.name)
								food.count = item.count
							// console.log('dec-shop', JSON.stringify(this.foods))
						})
					})
				}
			},
			// 清空购物车
			delAll() {
				this.goods.forEach((good) => {
					good.foods.forEach((food) => {
						if (food.count) {
							food.count = 0
							// console.log(JSON.stringify(food));
						}
					})
				})
			}
		}
	}
</script>

<style>
	.content {
		/* text-align: center; */
		/* height: 400upx; */
		display: flex;
		position: absolute;
		top: 0px;
		bottom: 55px;
		width: 100%;
		overflow: hidden;
	}

	.current {
		position: relative;
		z-index: 0;
		background-color: #fff;
		color: #00A0DC;
	}

	.menu-wrapper {
		text-align: center;
		width: 22%;
		display: flex;
		flex-direction: column;
		background: #f3f5f7;

	}

	.menu-item {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 50px;
	}

	.allcount {
		position: absolute;
		right: 6px;
		top: 8px;
		display: inline-block;
		padding: 0 4px;
		font-size: 9px;
		line-height: 16px;
		font-weight: 400;
		border-radius: 50%;
		background-color: #f01414;
		color: #ffffff;
	}

	.foods-wrapper {
		margin-left: 4px;
		width: 78%;
	}

	.food,
	.food-btm,
	.content {
		display: flex;
		flex-direction: row;
	}

	.food-title {
		padding: 2px 0;
	}

	.food-info {
		margin-left: 10px;
		margin-right: 16px;
		display: flex;
		flex-direction: column;
		flex: 2;
	}

	.food-btm {
		justify-content: space-between;

	}

	.food-price {
		color: #f01414;
		font-size: 16px;
	}

	.commentPage-header {
		height: 190rpx;
		display: flex;
		padding-bottom: 30rpx;
		border-bottom: 20rpx solid #F4F4F4;
	}

	.commentPage-score {
		flex: 3.5;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		align-items: center;
		font-size: 55rpx;
		color: #FFB001;
		border-right: 1px solid #E8E8E8;
	}

	.commentPage-score .overall {
		margin-top: 10rpx;
		font-size: 30rpx;
		color: #9D9D9D
	}

	.commentPage-score .rate {
		line-height: 50rpx;
		font-size: 25rpx;
		color: #E8E8E8;
	}

	.commentPage-rating {
		padding-top: 20rpx;
		flex: 6;
		display: flex;
		flex-direction: column;
		align-items: center;
		font-size: 30rpx;
		color: #9D9D9D;
	}

	.commentPage-rating .restaurant,
	.commentPage-rating .distribute {
		display: flex;
		width: 450rpx;
	}

	.commentPage-rating .distribute {
		margin-top: 40rpx;
	}

	.commentPage-rating .stars {
		margin: 0 40rpx;
	}

	.commentPage-category {
		display: flex;
		flex-wrap: wrap;
		border-bottom: 20rpx solid #F4F4F4;
	}

	.category-item-active {
		background: #F0AD4E;
		color: #007AFF;
	}

	.commentPage-category .category-item {
		height: 40rpx;
		line-height: 40rpx;
		margin: 20rpx 10rpx;
		font-size: 28rpx;
		color: #9D9D9D;
		border: 1px solid #ECECEC;
		border-radius: 40rpx;
		padding: 5rpx 15rpx;
	}

	page {
		background-color: #f8f8f8;
	}

	.uni-padding-wrap {
		padding: 30upx;
	}

	view {
		font-size: 28upx;
	}

	.uni-comment {
		padding: 5rpx 0;
		display: flex;
		flex-grow: 1;
		flex-direction: column;
	}

	.uni-comment-list {
		flex-wrap: nowrap;
		padding: 10rpx 0;
		margin: 10rpx 0;
		width: 100%;
		display: flex;
	}

	.uni-comment-face {
		width: 70upx;
		height: 80upx;
		border-radius: 100%;
		margin-right: 20upx;
		flex-shrink: 0;
		overflow: hidden;
	}

	.uni-comment-face image {
		width: 100%;
		border-radius: 100%;
	}

	.uni-comment-body {
		width: 100%;
	}

	.uni-comment-top {
		line-height: 1.5em;
		justify-content: space-between;
	}

	.uni-comment-top text {
		color: #0A98D5;
		font-size: 24upx;
	}

	.uni-comment-date {
		line-height: 38upx;
		flex-direction: row;
		justify-content: space-between;
		display: flex !important;
		flex-grow: 1;
	}

	.uni-comment-date view {
		color: #666666;
		font-size: 24upx;
		line-height: 38upx;
	}

	.uni-comment-content {
		line-height: 1.6em;
		font-size: 28upx;
		padding: 8rpx 0;
	}

	.uni-comment-replay-btn {
		background: #FFF;
		font-size: 24upx;
		line-height: 28upx;
		padding: 5rpx 20upx;
		border-radius: 30upx;
		color: #333 !important;
		margin: 0 10upx;
	}

	.anglesList {
		display: flex; //使用flex布局 
		align-items: center; //子元素在垂直方向居中对齐 
		padding: 25rpx 25rpx 25rpx 0; //留出边距 
		/* margin-left: 25rpx; //其实可以写在内边距中 */
		border-bottom: 1px solid #f1f1f1;
		background: #3F536E;
	}

	//anglesList的子元素指两个navigator 
	//头像和内容区域
	.anglesList .anglesNavigator {
		display: flex; //头像和内容区域启动flex布局 
		align-items: center; //子元素在垂直方向居中对齐 
		flex-grow: 1; //放大，占据剩余的空间 
	}

	//头像父层
	.anglesList .imgBox {
		flex: 0 0 110rpx; //flex 是flex-grow, flex-shrink 和 flex-basis的简写。flex:0 0 110rpx的意思是不放大 不缩小 固定宽度是110rpx 
		width: 110rpx;
		height: 110rpx;

		padding: 5rpx;
		margin-right: 20rpx;

	}

	//头像
	.anglesList .imgBox image {
		width: 110rpx;
		height: 110rpx;
		padding-left: 5px;

	}

	//文字区域
	.anglesList .textBox {
		flex-grow: 1; //放大，占据剩余的空间。 
	}

	//文本限制宽度，超出隐藏 
	.anglesList text {
		display: block;
		max-width: 12em;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		color: white;
	}

	//按钮 
	.btnFollowed {
		/* //flex-basis: 140rpx; //指定宽度，也可以写width */
		height: 55rpx;
		border: 1px solid #007ae8;
		border-radius: 6px;
		background: #F0AD4E;
		text-align: center;
		font-size: 28rpx;
		line-height: 55rpx;
		width: 40px;
		color: #007ae8;
	}

	.tag {
		display: flex;
		justify-content: flex-start;
		align-content: center;
	}

	@font-face {
		font-family: texticons;
		font-weight: normal;
		font-style: normal;
		src: url('https://at.alicdn.com/t/font_984210_5cs13ndgqsn.ttf') format('truetype');
	}

	.tag view {
		margin-right: 2px;
		width: auto;
		height: 45rpx;
		border: 1px dashed #007AFF;
		color: #F3F5F7;
	}

	.center {
		display: flex;
		flex-direction: column;
	}

	.center-list {
		display: flex;
		background-color: #FFFFFF;
		margin-top: 20upx;
		width: 750upx;
		flex-direction: column;
	}

	.center-list-item {
		display: flex;
		height: 90upx;
		width: 750upx;
		box-sizing: border-box;
		flex-direction: row;
		padding: 0upx 20upx;
	}

	.list-icon {
		display: flex;
		width: 40upx;
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #4cd964;
		text-align: center;
		font-family: texticons;
		margin-right: 20upx;
	}

	.list-text {
		display: flex;
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		flex: 1;
		text-align: left;
	}

	.navigat-arrow {
		display: flex;
		height: 90upx;
		width: 40upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		text-align: right;
		font-family: texticons;
	}

	.logo-hover {
		opacity: 0.8;
	}
</style>
