<template>
	<view>
		<view class="" v-if="!show">
			请先登录
		</view>
		<view v-if="show">
			<van-checkbox-group :value="result" @change="onChan">

				<van-swipe-cell :right-width="65" v-for="item in shopping" :key="item.product_id">
					<van-cell-group>
						<van-card :num="item.number" tag="热销" :price="item.price" :desc="item.volume" :title="item.title" :thumb="`${ip}/${item.image}`">
							<view slot="footer">
								<van-button size="mini" @click="reduce(item.product_id)">-</van-button>
								<van-button size="mini" @click="add(item.product_id)">+</van-button>
								<van-checkbox :name="item.product_id">选择</van-checkbox>
							</view>
						</van-card>
					</van-cell-group>
					<view class="delete" slot="right" @click="deleTe(item.product_id)">
						删除
					</view>
				</van-swipe-cell>
			</van-checkbox-group>
			<view class="s">

			</view>
		</view>
		<view class="">
			<van-submit-bar :price="prices" button-text="提交订单" @submit="submit()" tip="{{ true }}">
				<van-checkbox :value="onCh" @change="onChn()"></van-checkbox>
			</van-submit-bar>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 请求头
				ip: "http://localhost:8888/",
				// 请求数据
				shopping: [],
				// 复选框
				result: [],
				resul: [],
				onCh: false,
				// 金额
				prices: 0,
				// 页面显示
				show:false
			}
		},
		onTabItemTap() {
			// 获取
			const ures = wx.getStorageSync('username')
			if(ures === ''){
				this.show = false
			}else{
				this.show = true
				const value = uni.getStorageSync('proArr')
				if (value) {
					this.shopping = value
				}
			}
		},
		methods: {
			// 结算
			settlement() {
				this.prices = 0
				this.result.forEach(item => {
					for(var i=this.shopping.length-1;i>=0;i--){
						if(this.shopping[i].product_id === item){
							this.prices += this.shopping[i].price*this.shopping[i].number*100
						}
					}
				})
			},
			// 删除按钮
			deleTe(product_id) {
				const proArr = wx.getStorageSync('proArr')
				console.log(proArr)
				const index = proArr.findIndex(item => item.product_id === product_id)
				console.log(index)
				proArr.splice(index,1)
				wx.setStorageSync('proArr',proArr)
			},
			// 减
			reduce(product_id) {
				const proArr = wx.getStorageSync('proArr')
				const index = proArr.findIndex(item => item.product_id === product_id)
				proArr[index].number--
				wx.setStorageSync('proArr',proArr)
				this.shopping = wx.getStorageSync('proArr')
				this.settlement()
			},
			// 加
			add(product_id) {
				const proArr = wx.getStorageSync('proArr')
				const index = proArr.findIndex(item => item.product_id === product_id)
				proArr[index].number++
				wx.setStorageSync('proArr',proArr)
				this.shopping = wx.getStorageSync('proArr')
				this.settlement()
			},
			// 复选框
			onChan(event) {
				this.result = event.detail
				this.prices = 0
				if(this.shopping.length === this.result.length){
					this.onCh = true
				}else{
					this.onCh = false
					this.settlement()
				}
			},
			// 全选框
			onChn() {
				if (this.onCh) {
					this.onCh = false
					this.result = []
					this.prices = 0
				} else {
					this.onCh = true
					this.result = []
					this.shopping.forEach(item => {
						this.result.push(item.product_id)
					})
					this.settlement()
				}
			},
			// 提交
			submit(){
				this.prices = 0
				// console.log(1)
				this.result.forEach(item => {
					// console.log(item)
					for(var i=this.shopping.length-1;i>=0;i--){
						if(this.shopping[i].product_id === item){
							this.shopping.splice(i,1)
							break;
						}
					}
				})
				wx.setStorageSync('proArr',this.shopping)
			}
		}
	}
</script>

<style>
	/* 删除按钮 */
	.delete {
		text-align: center;
		background-color: #EE0A24;
		width: 204%;
		height: 100%;
		padding: 50px 0 0 0;
	}

	.s {
		height: 50px;
	}
</style>
