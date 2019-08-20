<template>
	<view class="">
		<Tabs :TabList="TabList" :currentTab="current" @tabs="tabsChange">
			<!-- 写页面 -->
			<TabPane>
				<view>
					<scroll-view class="container" scroll-y="true">
						<block v-for="(item,index) in orderList" :key="index">
							<view class="orderList">
								<view class="order-title">
									<view class="order-title-restaurantName">
										{{item.restaurantName}}</view>
									<view class="order-title-state">{{item.state}}</view>
								</view>
								<view class="order-content">
									<image class="order-content-restaurantImg" :src="item.src"></image>
									<view class="order-content-info">
										<view class="order-content-info-price">￥ {{item.price}}</view>
										<view class="order-content-info-date">
											{{item.time}}</view>
										<view class="order-content-info-howToDistribute">
											由 {{item.howToDistribute}} 提供服务</view>
									</view>
								</view>
								<view class="order-footer">
									<view class="order-footer-orderAgain" @click.stop="anotherList()">再来一单</view>
									<view class="order-footer-goToComment" @click.stop="comment()">去评论</view>
								</view>
							</view>
						</block>
					</scroll-view>
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
				<view>
					<scroll-view class="container" scroll-y="true">
						<block v-for="(item,index) in orderList" :key="index">
							<view class="orderList">
								<view class="order-title">
									<view class="order-title-restaurantName">
										{{item.restaurantName}}</view>
									<view class="order-title-state">{{item.state}}</view>
								</view>
								<view class="order-content">
									<image class="order-content-restaurantImg" :src="item.src"></image>
									<view class="order-content-info">
										<view class="order-content-info-price">￥ {{item.price}}</view>
										<view class="order-content-info-date">
											{{item.time}}</view>
										<view class="order-content-info-howToDistribute">
											由 {{item.howToDistribute}} 提供服务</view>
									</view>
								</view>
								<view class="order-footer">
									<view class="order-footer-orderAgain" @click.stop="anotherList()">再来一单</view>
									<view class="order-footer-goToComment" @click.stop="refund()">申请退款</view>
								</view>
							</view>
						</block>
					</scroll-view>
				</view>
			</TabPane>
		</Tabs>
	</view>
</template>
<script>
	import Tabs from '@/components/tabs.vue'
	import TabPane from '@/components/tabPane.vue'
	export default {
		data() {
			return {
				current: 0,
				active_flag:0,
				orderList: [],
				TabList: [{
						title: '全部订单'
					},
					{
						title: '待评价'
					},
					{
						title: '退款'
					}
				],
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
				}]
			}
		},
		methods: {
			tabsChange(index) {
				this.current = index
			},
			category_active: function(e) {
				var that=this;
				that.active_flag=e.currentTarget.dataset.id;
				console.log(e.currentTarget.dataset.id);
			},
			getOrder: function() {
				var that = this;
				uni.request({
					url: "https://www.easy-mock.com/mock/5d3726935317be31dd341b39/takeOut/Oder",
					method: "GET",
					success: function(res) {
						console.log(res);
						for (let x of res.data.data.order) {
							that.orderList.push(x);
						}
						uni.hideLoading();
					}
				})
			},
			anotherList: function() {
				uni.showToast({
					icon: "none",
					title: "癞蛤蟆想吃天鹅肉",
					duration: 2000,
					position: "bottom"
				})
			},
			refund:function(){
				uni.showToast({
					icon: "none",
					title: "下单概不退款!",
					duration: 2000,
					position: "bottom"
				})
			},
			comment: function() {
				uni.showToast({
					icon: "none",
					title: "消费太少,无法评论!",
					duration: 2000,
					position: "bottom"
				})
			}
		},
		components: {
			Tabs,
			TabPane
		},
		onReady() {
			uni.showLoading({
				title: "加载中...",
				mask: true
			})
			this.getOrder();
		}
	}
</script>
<style>
	.container {
		height: 100%;
	}

	.orderList {
		width: 100%;
		padding: 15rpx;
		border: 100rpx 0;
		border-style: solid;
		border-color: #ECECEC;
	}

	.order-title {
		width: 100%;
		height: 70rpx;
		line-height: 70rpx;
		border-bottom: 1rpx solid #ECECEC;
		display: flex;
		justify-content: space-between;
	}

	.order-title-restaurantName {
		width: 200rpx;
		height: 100%;
		font-size: 30rpx;
	}

	.order-title-state {
		width: 130rpx;
		color: #D2D2D2;
		margin-right: 20rpx;
		font-size: 28rpx;
	}

	.order-content {
		height: 220rpx;
		display: flex;
		align-items: center;
		border-bottom: 1rpx solid #ECECEC;
	}

	.order-content-restaurantImg {
		width: 200rpx;
		height: 125rpx;
	}

	.order-content-info {
		font-size: 25rpx;
		flex: 1;
		display: flex;
		flex-direction: column;
	}

	.order-content-info-price,
	.order-content-info-date,
	.order-content-info-howToDistribute {
		height: 42rpx;
		line-height: 42rpx;
		margin-left: 30rpx;
	}

	.order-content-info-price {
		color: red;
		font-size: 30rpx;
	}

	.order-footer {
		width: 100%;
		height: 90rpx;
		display: flex;
		justify-content: flex-end;
		font-size: 28rpx;
	}

	.order-footer-goToComment,
	.order-footer-orderAgain {
		margin-top: 15rpx;
		margin-right: 40rpx;
		height: 70rpx;
		line-height: 70rpx;
		width: 200rpx;
		text-align: center;
		border: 1rpx solid #ECECEC;
	}

	.order-footer-orderAgain {
		margin-right: 20rpx;
	}

	.order-footer-goToComment {
		background: #FFD161;
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
</style>
