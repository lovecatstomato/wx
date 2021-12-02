<template>
	<view>
		<view class="overall">
			<view>
				<van-sidebar :active-key="0">
					<van-sidebar-item v-for="item in titleTiel" @click="onChange(item.id)" :key="item.id" :title="item.name" />
				</van-sidebar>
			</view>
			
			<view>
				<view class="titleAll" v-for="(itemGroup,index) in pageContent" :key="index">
					<text class="titleText">{{itemGroup.className.name}}</text>
					<view class="imgsAll">
						<view class="imgsTe" v-for="(content,index) in itemGroup.classGroup" :key="index">
							<image class="imgs" :src="`${IpId}/${content.class_photo}`"></image>
							<text class="Text">{{content.name}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				// id请求头
				IpId: "http://localhost:8888",
				// 标题数组
				titleTiel: [],
				// 标题获取页面数组
				// 内容
				pageContent: []
			}
		},
		onTabItemTap() {
			// 请求数据
			uni.request({
				url: `${this.IpId}/data/category_list.json`,
				success: res => {
					this.titleTiel = res.data
					// console.log(res)
				},
			});
			uni.request({
				url: `${this.IpId}/data/category_list_478.json`,
				success: res => {
					// 内容
					this.pageContent = res.data
					// console.log(this.pageContent)
				}
			});
		},
		methods: {
			// 按钮获取数据
			onChange(id) {
				uni.request({
					url: `${this.IpId}/data/category_list_${id}.json`,
					success: res => {
						// 内容
						this.pageContent = res.data
						// console.log(this.pageContent)
					}
				});
			}
		}
	}
</script>

<style>
	/* 主体框架 */
	.overall {
		display: flex;
	}
	/* 主体框架 */
	.titleAll{
		text-align: center;
		box-shadow: 6px 6px 6px #F4F4F4;
		width: 235px;
		padding: 10px 0 10px 0;
	}
	/* 列表标题 */
	.titleText{
		font-size: 18px;
		font-weight: bold;
	}
	.imgsTe{
		padding: 5px;
	}
	/* 图片主体框架 */
	.imgsAll {
		display: grid;
		grid-template-columns: 80px 80px 80px;
		/* text-align: center; */
	}

	/* 图片 */
	.imgs {
		height: 70px;
		width: 70px;
	}
	/* 内容介绍 */
	.Text{
		font-size: 14px;
	}
</style>
