<template>
	<view class="">
		<van-toast id="van-toast" />
		<!-- 轮播图 -->
		<view class="Carousel">
			<swiper autoplay="true" style="height: 348px;">
				<block v-for="item in commodityData.templatePhoto" :key="index">
					<swiper-item>
						<image mode="widthFix" :src="`${ip}/${item}`"></image>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<!-- 商品介绍 -->
		<view class="introduce">
			<view class="titleText">
				<p class="ti">{{title}}</p>
				<p class="ti-desc">{{produInfo.product_desc}}</p>
				<view class="ti-all">
					<span>￥</span>
					<span class="ti-price">{{produInfo.price}}</span>
				</view>
			</view>
			<view class="tia">
				<view v-for="item in commodityData.productItem" :key="item.id" class="ti-volume">
					<span>{{item.volume}}</span>
				</view>
			</view>
			<view class="sendTime">
				<span>{{commodityData.sendTimeMsg}}</span>
			</view>
			<view class="refund">
				<span>{{commodityData.refundRule}} ></span>
			</view>
		</view>
		<!-- 相似商品 -->
		<view class="Similar">
			<p class="similarText">相似商品</p>
			<view class="prod">
				<view v-for="items in commodityData.productItem" :key="items.id">
					<view class="">
						<span>￥</span>
						<span>{{items.price}}</span>
						<span>/{{items.volume}}</span>
						<image class="butTon" :src="`${ip}/images/cart.png`" />
					</view>
				</view>
			</view>

		</view>
		<!-- 商品注意事项 -->
		<view class="">
			<table class="customers">
				<tr>
					<td>{{tempInfo.origin}}</td>
					<td>{{tempInfo.originMsg}}</td>
				</tr>
				<tr>
					<td>{{tempInfo.explain}}</td>
					<td>{{tempInfo.explainMsg}}</td>
				</tr>
				<tr>
					<td>{{tempInfo.store}}</td>
					<td>{{tempInfo.storeMsg}}</td>
				</tr>
			</table>
		</view>
		<!-- 商品介绍图片 -->
		<view class="">
			<view v-for="item in commodityData.templateInfo.desc_imgs" :key="index">
				<image mode="widthFix" :src="`${ip}/${item}`"></image>
			</view>
		</view>
		<van-goods-action>
			<van-goods-action-icon icon="chat-o" text="客服" />
			<van-goods-action-icon icon="cart-o" text="购物车" />
			<van-goods-action-button @click="onClickButton()" color="#FF8A00" text="加入购物车" type="warning" />
			<van-goods-action-button @click="Buy()" color="#FFC70A" text="立即购买" />
		</van-goods-action>
	</view>




</template>

<script>
	import Toast from "../../static/vant/toast/toast"
	export default {
		data() {
			return {
				// 标题
				title: "",
				// 请求头
				ip: "http://localhost:8888",
				// 数据数组
				commodityData: [],
				// 详细标题数组
				produInfo: [],
				// 注意事项数组
				tempInfo: []
			}
		},
		onLoad(option) {
			console.log(option.id)
			// 请求数据
			uni.request({
				url: `${this.ip}/data/good_detail_${option.id}.json`,
				success: res => {
					console.log(res.data)
					// 主要数组
					this.commodityData = res.data
					// 标题数组
					this.produInfo = this.commodityData.productInfo
					// 标题名字
					this.title = this.commodityData.productInfo.product_name.substring(0, 6)
					// 注意事项
					this.tempInfo = this.commodityData.templateInfo.pro_info
				},
			});
		},
		methods: {
			// 加入购物车
			onClickButton() {
				Toast('加入购物车成功')
			},
			// 立即购买
			Buy() {
				Toast('购买成功')
			}
		}
	}
</script>

<style>
	/* 介绍 */
	.introduce {
		position: relative;
		left: 20px;
		width: 260px;
		bottom: 60px;
		padding: 10px;
		box-shadow: 1px 1px 1px 1px #F4F4F4;
		border-radius: 10px;
		background-color: #ffffff;
	}

	/* 标题框架 */
	.titleText {
		text-align: center;
	}

	/* 标题 */
	.ti {
		padding: 8px;
		font-weight: bold;
	}

	/* 标题介绍 */
	.ti-desc {
		color: #B7B7B7;
		padding-bottom: 15px;
	}

	/* 价格框架 */
	.ti-all {
		color: #FF7B00;
		font-weight: bolder;
	}

	/* 价格 */
	.ti-price {
		font-size: 24px;
	}

	.tia {
		display: grid;
		grid-template-columns: 80px 80px;
		margin: auto;
		width: 190px;
	}

	.ti-volume {
		white-space: nowrap;
		border-radius: 10px;
		box-shadow: 1px 1px 1px 1px #F4F4F4;
		padding: 8px;
		margin: 5px;
		text-align: center;
		font-size: 14px;
		font-weight: bold;
	}

	.tia view:nth-child(2) {
		width: 80px;
		background-color: #FF8900;
		color: #ffffff;
	}

	/* 送达时间 */
	.sendTime {
		font-size: 12px;
		text-align: center;
		color: #C9C9C9;
	}

	/* 送达标准 */
	.refund {
		padding: 16px 0 0 0;
		font-size: 16px;
		text-align: center;
		font-weight: bold;
	}

	/* 注意事项表格 */
	.customers {
		font-family: Arial, Helvetica, sans-serif;
		border-collapse: collapse;
		width: 100%;
	}

	.customers td,
	.customers th {
		border: 1px solid #ddd;
		padding: 8px;
		width: 100%;
	}

	.customers tr {
		display: flex;
	}
	/* 相似商品主框架 */
	.Similar{
		position: relative;
		left: 20px;
		width: 280px;
		bottom: 40px;
		box-shadow: 1px 1px 1px 1px #F4F4F4;
		border-radius: 10px;
		background-color: #ffffff;
	}
	/* 相似商品文字标题 */
	.similarText{
		font-weight: bold;
	}
	/* 相似商品框架 */
	.prod {
		display: flex;
		font-size: 12px;
		font-weight: bold;
		padding: 10px;
	}

	/* 相似商品按钮 */
	.butTon {
		width: 20px;
		height: 20px;
	}
</style>
