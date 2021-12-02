<template>
	<view>
		<view>
			<van-swipe-cell right-width="{{65}}">
				<van-cell-group>
					<van-card num="2" tag="标签" price="10.00" desc="描述信息" title="商品标题" thumb="{{ imageURL }}">
						<view slot="footer">
							<van-button size="mini">按钮</van-button>
							<van-button size="mini">按钮</van-button>
						</view>
					</van-card>
				</van-cell-group>
				<view class="" slot="right">
					删除
				</view>
			</van-swipe-cell>
		</view>
		<view class="">
			<van-submit-bar price="{{ 3050 }}" button-text="提交订单" tip="{{ true }}">
				<van-tag type="primary">标签</van-tag>
			</van-submit-bar>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shopping: [],
			}
		},
		onTabItemTap() {
			const value = uni.getStorageSync('joinInd')
			if(value){
				this.shopping = value
				console.log(this.shopping)
			}
				
		}
	}
</script>

<style>
</style>
