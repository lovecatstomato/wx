<template>
	<view>
		<van-tabs :swipe-threshold="4">
			<van-tab v-for="item in New" class="titleText" :title="item.name" :key="item.id">
				<view class="filter">
					<span>综合</span>
					<span>销量</span>
					<span>价格</span>
				</view>
				<view v-for="items in item.productGroup" :key="items.id">
					<view class="imgsAll">
						<image class="imgs" :src="`${ip}/${items.photo}`"></image>
						<view class="textAll">
							<p>{{items.product_name}}</p>
							<p>{{items.product_desc}}</p>
							<p class="textPrice">
								<p class="volume">{{items.volume}}</p>
								<span style="color: #FF7E00">￥</span>
								<span id="price">{{items.price}}</span>
								<van-tag class="Loan" color="#FF7E00" text-color="#ffe1e1">明日达</van-tag>
								<image class="butTon" :src="`${ip}/images/cart.png`"></image>
							</p>
						</view>
					</view>
					<van-divider />
				</view>
			</van-tab>
		</van-tabs>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 请求头
				ip: "http://localhost:8888",
				// 请求数组
				New: []
			}
		},
		onLoad() {
			uni.request({
				url: `${this.ip}/data/sub_category_list_0.json`,
				success: res => {
					this.New = res.data
					console.log(this.New)
				}
			});
		}
	}
</script>

<style>
	/* 图片 */
	.imgs {
		width: 100px;
		height: 100px;
	}

	/* 图片主体框架 */
	.imgsAll {
		display: grid;
		grid-template-columns: 100px 220px;
		font-size: 12px;
	}

	/* 图片内文字主体框架 */
	.textAll {
		padding-left: 10px;
		color: #ADADAD;
	}

	/* 价格框架 */
	.textPrice {
		margin: 30px 0 0 0;
	}

	/* 单位 */
	.volume {
		font-weight: bold;
	}

	/* 价格 */
	#price {
		color: #FF7E00;
		font-size: 20px;
		font-weight: bold;
	}

	/* 标签 */
	.Loan {
		padding-left: 10px;
	}

	/* 添加按钮 */
	.butTon {
		float: right;
		height: 30px;
		width: 30px;
	}

	/* 筛选按钮 */
	.filter {
		display: grid;
		text-align: center;
		grid-template-columns: 33.3% 33.3% 33.3%;
		padding: 10px;
		background: #F5F5F5;
		margin-bottom: 10px;
	}
</style>
