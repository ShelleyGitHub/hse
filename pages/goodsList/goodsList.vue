<template>
	<view class="warp">
		<!-- #ifdef APP-PLUS -->
		<statusBar></statusBar>
		<!-- #endif -->
		<!-- 筛选栏 -->
		<view class="selectWrap">
			<!-- <u-navbar
						title="选择器"
						@leftClick="navigateBack"
						safeAreaInsetTop
						fixed
						placeholder
					></u-navbar> -->
			<u-cell-group>
				<u-cell @click="showPicker(index)" :title="item.title" v-for="(item, index) in list" :key="index"
					isLink>
				</u-cell>
			</u-cell-group>
			<u-picker
				:show="show1"
				:columns="columns1"
				closeOnClickOverlay
				@cancel="cancel"
				@confirm="confirm"
				@close="close"
				@change="change"
			></u-picker>
			<u-picker
				:show="show2"
				:columns="columns2"
				closeOnClickOverlay
				@cancel="cancel"
				@confirm="confirm"
				@close="close"
				@change="change"
			></u-picker>
			<u-picker
				:show="show3"
				:columns="columns3"
				closeOnClickOverlay
				@cancel="cancel"
				@confirm="confirm"
				@close="close"
				@change="change"
			></u-picker>
			<u-picker
				:show="show4"
				:columns="columns4"
				closeOnClickOverlay
				@cancel="cancel"
				@confirm="confirm"
				@close="close"
				@change="changeHandler2"
				ref="uPicker4"
			></u-picker>
		</view>
		<view class="listWrap">
			<view class="listOne" v-for="(item,index) in goodsList" :key="index">
				<view class="goodsPic">
					<image :src="item.goods_thumb" mode="scaleToFill" class="pic"></image>
				</view>
				<view class="goodsName">{{item.name}}</view>
			</view>

		</view>
	</view>
</template>

<script>
	import statusBar from "@/uni_modules/uni-nav-bar/components/uni-nav-bar/uni-status-bar";
	const db = uniCloud.database();
	const dbCmd = db.command;
	export default {
		components: {
			statusBar
		},
		data() {
			return {
				filterDropdownValue: 0,
				loading:false,
				navList: [],
				goodsList: [],
				columnData: [
					['全部', '北郊', '育德', '白石'],
					['全部', '江海1', '江海2', '江海3', '江海4']
				],
				list: [{
						title: '房屋种类',
						// iconUrl: 'https://cdn.uviewui.com/uview/demo/picker/2.png'
					},
					{
						title: '房屋特点',
						// iconUrl: 'https://cdn.uviewui.com/uview/demo/picker/5.png'
					},
					{
						title: '售价',
						// iconUrl: 'https://cdn.uviewui.com/uview/demo/picker/1.png'
					},
					{
						title: '区域',
						// iconUrl: 'https://cdn.uviewui.com/uview/demo/picker/3.png'
					}
				],
				show: true,
				columns1: [
					['不限','新房', '二手房','商铺'],
				],
				columns2: [
					['不限','低单价', '低容积', '主卧套间']
				],
				columns3: [
					['不限','30万以下','30-40万','40-50万']
				],
				columns4: [
					['蓬江区', '江海区'],
					['不限', '北郊', '育德', '白石']
				],
				show1: false,
				show2: false,
				show3: false,
				show4: false

			};
		},
		onLoad() {
			this.getNav();
			this.getList();
		},
		methods: {
			change(e) {
				// console.log('change', e);
			},
			changeHandler2(e) {
				this.change(e)
				const {
					columnIndex,
					value,
					values,
					index,
					// 微信小程序无法将picker实例传出来，只能通过ref操作
					picker = this.$refs.uPicker4
				} = e
				if (columnIndex === 0) {
					// this.loading = true
					uni.$u.sleep(1500).then(() => {
						picker.setColumnValues(1, this.columnData[index])
						this.loading = false
					})
				}
			},
			confirm(e) {
				this.value = e.value[e.value.length-1];
			    this.list.splice(this.index-1,1,{'title':this.value});
				this[`show${this.index}`] = false
			},
			cancel() {
				// console.log('cancel');
				this[`show${this.index}`] = false
			},
			showPicker(index) {
				this.index = index + 1
				this[`show${index + 1}`] = true
			},
			close() {
				// console.log('close');
				this[`show${this.index}`] = false
			},
			// 获取分类
			async getNav() {
				let res = await db.collection("house-nav").get();
				this.navList = res.result.data;
			},
			getList() {
				uniCloud.callFunction({
					name: "house_get_goods"
				}).then(res => {
					this.goodsList = res.result.data;
					console.log(this.goodsList, '000')
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.warp {
		padding-top: 46rpx;
		height: 100vh;
		overflow-y: auto;
		// background: #e5e5e5;
		.u-page {
			padding: 0;
		}
		
		.selectWrap{
			background:#fff;
			margin-bottom:20rpx;
			/deep/ .u-cell-group{
				flex-direction: row !important;
				position:fixed;
				left:0;
				top:0;
				width:100vw;
				height:100rpx;
				z-index: 1;
				background-color: #fff;
				.u-cell-group__wrapper{
					flex-direction: row !important;
					.u-line{
						display: none !important;
					}
					.u-cell{
						flex-direction: row !important;
					}
				}
			}
			/deep/ .u-picker{
				.u-picker__view{
					height:50vh !important;
				}
			}
		}
	}

	.select1 {
		margin-top: 50rpx;
	}

	.listWrap {
		width: 92vw;
		margin: 0 auto;
		overflow: hidden;
		margin-top:100rpx;
		// border:1px solid red;
		// box-sizing: border-box;
		.listOne {
			width: 44vw;
			// height:45vw;
			padding-bottom: 50rpx;
			box-sizing: border-box;
			margin-bottom: 4vw;
			float: left;
			background-color: #fff;
			border-radius: 20rpx;
			overflow: hidden;

			&:nth-of-type(odd) {
				margin-right: 4vw;
			}

			.goodsPic {
				width: 100%;
				height: 45vw;
				margin: 0 auto;

				.pic {
					width: 100%;
					height: 100%;
				}
			}

			.goodsName {
				color: #333;
				text-align: center;
				line-height: 30rpx;
				font-size: 30rpx;
				margin-top: 20rpx;
			}
		}
	}
</style>
