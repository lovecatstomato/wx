<template>
	<view>
		<!-- 轮播图 -->
		<van-dialog id="van-dialog" />
		<view class="Carousel">
			<swiper indicator-dots="true" autoplay="true">
				<block v-for="item in overall" :key="index">
					<swiper-item>
						<image :src="`${IpId}${item}`"></image>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<!-- 导航栏 -->
		<view>
			<van-grid column-num="5" :border="false">
				<van-grid-item @click="NewEvery()" :icon="`${IpId}images/week.png`" text="每周上新" />
				<van-grid-item :icon="`${IpId}images/new.png`" text="新客专享" />
				<van-grid-item :icon="`${IpId}images/play.png`" text="天天直播" />
				<van-grid-item :icon="`${IpId}images/shake.png`" text="摇一摇" />
				<van-grid-item :icon="`${IpId}images/limit.png`" text="限时特惠" />
				<van-grid-item :icon="`${IpId}images/low.png`" text="低温速食" />
				<van-grid-item :icon="`${IpId}images/meat.png`" text="品质肉禽" />
				<van-grid-item @click="Newfruits()" :icon="`${IpId}images/fruit.png`" text="新鲜水果" />
				<van-grid-item :icon="`${IpId}images/gh.png`" text="粮油干货" />
				<van-grid-item :icon="`${IpId}images/water.png`" text="生活水饮" />
			</van-grid>
		</view>
		<!-- 热销榜 -->
		<view>
			<view v-for="item in commodity" :key="item.id">
				<!-- key值对所需数据进行定位，不要使用index索引，使用请求过来里面的id值 -->
				<image class="tiel" :src="`${IpId}${item.image}`" />
				<movable-area class="imgsPic">
					<movable-view class="imgsAll" direction="horizontal">
						<view class="imgs" v-for="ie in item.content" :key="ie.product_id">
							<image @click="productId(ie.product_id,ie.image)" class="picture" :src="`${IpId}${ie.image}`" mode="" />
							<text class="imgtitel">{{ie.subtitle}}</text>
							<text class="imgIntroduce">{{ie.title}}</text>
							<view class="getRid">
								<text class="price">￥{{ie.price}}</text>
								<text class="volume">/{{ie.volume}}</text>
								<image @click="joinIn(ie)" class="butTon" :src="`${IpId}images/cart.png`" />
							</view>
						</view>
					</movable-view>
				</movable-area>
			</view>
		</view>

	</view>


</template>

<script>
	import Toast from '../../static/vant/toast/toast';
	import Dialog from '../../static/vant/dialog/dialog';
	export default {
		data() {
			return {
				// id请求头
				IpId: "http://localhost:8888/",
				// 轮播图数组
				overall: [],
				// 商品热销数组
				commodity: [],
			}
		},
		onLoad() {
			// 请求数据
			uni.request({
				url: `${this.IpId}data/fruits.json`,
				success: res => {
					this.overall = res.data.banners
					this.commodity = res.data.bannerTags
					console.log(this.commodity)
				},
			});
		},
		methods: {
			// 每周上新
			NewEvery() {
				uni.navigateTo({
					url: '../week/week'
				});
			},
			// 新鲜水果
			Newfruits() {
				uni.navigateTo({
					url: '../week/fruits'
				});
			},
			// 详细页面请求
			productId(id,images) {
				// console.log(id)
				// console.log(images)
				uni.navigateTo({
					url: `../Detailpage/Detailpage?id=${id}&image=${images}`
				});
			},
			// 加入购物车
			joinIn(join) {
				const user = wx.getStorageSync('username')
				if (user) {
					const proObj = {
						title: join.title,
						product_id: join.product_id,
						price: join.price,
						volume: join.volume,
						image: join.image,
						number: 1,
						checked: false
					}
					if (!wx.getStorageSync('proArr')) {
						const proArr = []
						proArr.push(proObj)
						wx.setStorageSync('proArr', proArr)
					} else {
						const proArr = wx.getStorageSync('proArr')
						const index = proArr.findIndex(item => item.product_id === join.product_id)
						if (index === -1) {
							proArr.push(proObj)
						} else {
							proArr[index].number++
						}
						wx.setStorageSync('proArr', proArr)

					}
					this.proArr = wx.getStorageSync('proArr')
				} else {
					Dialog.alert({
						message: '请先登录',
					})
				}
			}

		}
	}
</script>

<style>
	/* 轮播图图片 */
	.Carousel image {
		height: 100%;
		width: 100%;
	}

	/* 轮播标题 */
	.tiel {
		height: 230rpx;
	}

	/* 图片主体框架 */
	.imgsPic {
		height: 200px;
		padding: 10px;
	}

	/* 图片包含小框架 */
	.imgsAll {
		display: flex;
		width: 2000px;
	}

	/* 图片小框架 */
	.imgs {
		padding: 5px;
		width: 300rpx;
	}

	/* 图片详细标题 */
	.imgtitel {
		font-weight: bolder;
		font-size: 15px;
		display: inline-block;
		white-space: nowrap;
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	/* 详细图片大小 */
	.picture {
		height: 300rpx;
		width: 300rpx;
	}

	/* 图片详细介绍 */
	.imgIntroduce {
		font-size: 14px;
		color: #707070;
		text-align: center;
		display: inline-block;
		white-space: nowrap;
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	/* 详细价格 */
	.price {
		color: #EDA862;
		font-weight: bold;
		font-size: 16px;
	}

	.getRid {
		display: flex;
		align-items: center;
	}

	/* 详细单位 */
	.volume {
		color: #EDA862;
		font-weight: bold;
		font-size: 12px;
	}

	.butTon {
		width: 30px;
		height: 30px;
	}
</style>
