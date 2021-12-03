<template>
	<view>
		<view class="user">
			<view class="user-image">
				<van-image round width="4rem" height="4rem" src="{{ures.avatarUrl}}" />
				<span v-if="!ures">
					<p class="usename" @click="log()">
						登录注册
					</p>
					<p class="sign">
						{{name.Sign}}
					</p>
				</span>
				<span v-else>
					<p class="usename">
						{{ures.nickName}}
					</p>
					<p class="sign">
						{{name.Sign}}
					</p>
				</span>
				<span class="quit" @click="quit">退出登录</span>
			</view>
			<view class="user-assets">
				<table class="tabLe">
					<tr>
						<td>积分</td>
						<td>资产·充值</td>
						<td>优惠劵</td>
						<td>礼品</td>
					</tr>
					<tr>
						<td>-</td>
						<td>-</td>
						<td>-</td>
						<td>-</td>
					</tr>
				</table>
			</view>
		</view>
		<view class="mdon">
			<view class="Order">
				<span>我的订单</span>
				<span>全部订单></span>
			</view>
			<view class="Money">
				<van-grid :border="false" column-num="5">
					<van-grid-item icon="coupon-o" text="待付款" />
					<van-grid-item icon="gift-o" text="代发货" />
					<van-grid-item icon="balance-list-o" text="待收货" />
					<van-grid-item icon="gift-card-o" text="待评价" />
					<van-grid-item icon="send-gift-o" text="退换货" />
				</van-grid>
			</view>
		</view>
		<view class="RechVip">
			<view class="Recharge">
				<span>充值有礼</span>
				<span>充值中心></span>
			</view>
			<view class="vip">
				<span>$</span>
				<span>20000</span>
				<p>送价值89.90元赠品</p>
			</view>
		</view>
		<view class="comprehensive">
			<van-grid :border="false">
				<van-grid-item icon="coupon-o" text="会员中心" />
				<van-grid-item icon="gift-o" text="直播间" />
				<van-grid-item icon="balance-list-o" text="天天新鲜屋" />
				<van-grid-item icon="gift-card-o" text="卡券中心" />
				<van-grid-item icon="send-gift-o" text="企业购" />
				<van-grid-item icon="phone-o" text="在线客服" />
				<van-grid-item icon="diamond-o" text="我的问答" />
			</van-grid>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 用户数据
				ures: '',
				name: {
					use: '登录/注册',
					Sign: '签到送好礼'
				},
				uen:false
			}
		},
		onLoad() {
			// 推荐使用wx.getUserProfile获取用户信息，开发者每次通过该接口获取用户个人信息均需用户确认
			// 开发者妥善保管用户快速填写的头像昵称，避免重复弹窗
			uni.removeStorage({
			    key: 'username',
			    success:(res)=>{
			        this.ures = ''
			    }
			});

		},
		methods: {
			// 登录
			log() {
				wx.getUserProfile({
					desc: '用于完善会员资料', // 声明获取用户个人信息后的用途，后续会展示在弹窗中，请谨慎填写
					success: (res) => {
						this.ures = res.userInfo
						wx.setStorageSync('username', res.userInfo)
					}
				})
			},
			// 退出登录
			quit(){
				uni.removeStorage({
				    key: 'username',
				    success:(res)=>{
				        this.ures = ''
				    }
				});
			}
		}
	}
</script>

<style>
	/* 用户信息框架 */
	.user {
		margin: 10px;
		border-radius: 13px;
		background-color: #F2F2F2;
	}

	/* 用户信息 */
	.user-image {
		display: flex;
		padding: 10px;
	}

	.user-image span {}

	/* 用户资产 */
	.user-assets {}

	/* 表格 */
	.tabLe {
		padding: 10px 15px 25px 15px;
		font-size: 17px;
	}

	.tabLe tr {
		display: flex;
		justify-content: space-around;
		font-weight: bold;
		color: #6A6967;
	}

	/* 订单框架 */
	.mdon {
		padding: 5px;
		margin: 10px;
		box-shadow: 6px 6px 31px 7px #F4F4F4;
		border-radius: 14px;
	}

	/* 订单文字 */
	.Order {
		display: flex;
		padding: 10px;
		justify-content: space-between;
	}

	/* 货款 */
	.Money {}

	/* 充值框架 */
	.RechVip {
		padding: 10px;
		background: #ffffff;
		box-shadow: 6px 6px 31px 7px #F4F4F4;
		border-radius: 14px;
		margin: 10px;
	}

	/* 充值 */
	.Recharge {
		display: flex;
		justify-content: space-between;
		padding: 10px;
	}

	/* 价格 */
	.vip {
		background: #F5F5F5;
		font-weight: bold;
		margin: 0 100px 0 10px;
		padding: 13px;
		border-radius: 12px;
		width: 155px;
		height: 68px;
	}

	.vip p {
		color: #CCA768;
		font-size: 14px;
	}

	.vip span:nth-child(2) {
		font-size: 26px;
	}

	/* 综合 */
	.comprehensive {}

	/* 用户名 */
	.usename {
		margin: 4px;
		font-weight: bold;
		font-size: 20px;
	}

	/* 签到 */
	.sign {
		margin: 4px;
		background-color: #FB8C00;
		color: #ffff;
		font-weight: bold;
		padding: 3px;
		border-radius: 7px;
	}

	/* 退出登录 */
	.quit {
		width: 66px;
		height: 22px;
		margin: 19px 0 0 30px;
		padding: 5px;
		background-color: #C2C2C2;
		border-radius: 10px;
		color: #888888;
	}
</style>
