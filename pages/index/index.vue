<template>
	<view>
		<view class="search-wrap"><u-search placeholder="日照香炉生紫烟" v-model="keyword"></u-search></view>
		<view class="uni-margin-wrap">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" indicator-active-color="#221a21" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item>
					<view class="swiper-item"><image src="../../static/images/1.jpg" mode="aspectFill" class="image"></image></view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item"><image src="../../static/images/2.jpg" mode="aspectFill" class="image"></image></view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item"><image src="../../static/images/3.jpg" mode="aspectFill" class="image"></image></view>
				</swiper-item>
			</swiper>
		</view>
		<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll">
			<view class="scroll-view-item_H" v-for="(nav,index) in navList" :key="index">
				<view class="icon_nav">
					<!-- <i class="iconfont icon-home"></i> -->
					<image :src="nav.icon" mode="aspectFit" class="icon_pic"></image>
				</view>
				<text class="icon_text">{{nav.name}}</text>
			</view>

			<!-- <view class="scroll-view-item_H">
				<view class="icon_nav">
					<i class="iconfont icon-bank"></i>
				</view>
				<text class="icon_text">二手房</text>
			</view>
			<view class="scroll-view-item_H">
				<view class="icon_nav">
					<i class="iconfont icon-shop"></i>
				</view>
				<text class="icon_text">商铺</text>
			</view>
			<view class="scroll-view-item_H">
				<view class="icon_nav">
					<i class="iconfont icon-home"></i>
				</view>
				<text class="icon_text">新房</text>
			</view> -->
		</scroll-view>
		<view class="attr_wrap">
			<view class="attr_one">楼盘热榜</view>
			<view class="attr_one">低单价</view>
			<view class="attr_one">低容积率</view>
			<view class="attr_one">主卧套间</view>
		</view>
	</view>
</template>

<script>
	const db = uniCloud.database();
	const dbCmd = db.command;
	export default {
		data() {
			return {
				keyword:"",
				// background: ['color1', 'color2', 'color3'],
				indicatorDots: true,
				autoplay: true,
				interval: 5000,
				duration: 500,
				scrollTop: 0,
				old: {
					scrollTop: 0
				},
				navList:[]
			}
		},
		onLoad() {
			this.getNav();;
			this.getAttr();
		},
		methods:{
			// 获取分类
			async getNav(){
				let res = await db.collection("house-nav").get();	
				this.navList = res.result.data;
			},
			async getAttr(){
				let res = await db.collection("house-attr").get();	
				
			},
			// 获取产品
			async getGoods(){
				let res = await db.collection("house-goods").get();	
				// this.goodsList = res.result.data;
				// console.log(this.goodsList,'12222')
			},
			// swiper
			changeIndicatorDots(e) {
				this.indicatorDots = !this.indicatorDots
			},
			changeAutoplay(e) {
				this.autoplay = !this.autoplay
			},
			intervalChange(e) {
				this.interval = e.target.value
			},
			durationChange(e) {
				this.duration = e.target.value
			},
			// scroll-view
			upper: function(e) {
				console.log(e)
			},
			lower: function(e) {
				console.log(e)
			},
			scroll: function(e) {
				console.log(e)
				this.old.scrollTop = e.detail.scrollTop
			},
			goTop: function(e) {
				// 解决view层不同步的问题
				this.scrollTop = this.old.scrollTop
				this.$nextTick(function() {
					this.scrollTop = 0
				});
				uni.showToast({
					icon: "none",
					title: "纵向滚动 scrollTop 值已被修改为 0"
				})
			}
		}
	}
</script>

<style lang="scss">
@import "@/uni_modules/uview-ui/index.scss";
// swiper
.search-wrap{
	padding:30rpx 10rpx;
	box-sizing: border-box;
}
.uni-margin-wrap {
	width: 100%;
	
	.swiper {
		height:484rpx;
	}
	.swiper-item {
		display: block;
		text-align: center;
	}
	.image{
		width:100%;
	}
	.uni-swiper .uni-swiper-dot-active{
		background-color: #fff !important;
	}
}

// scroll_view
.scroll-view_H {
	white-space: nowrap;
	width: 100%;
	margin-top:40rpx;
	.uni-scroll-view-content{
		display:flex;
	}
	.scroll-view-item {
		height: 300rpx;
		line-height: 300rpx;
		text-align: center;
		font-size: 36rpx;
		
	}
	.scroll-view-item_H {
		display: inline-block;
		width: 33.3vw;
		height: 35vw;
		text-align: center;
	}
	 .icon_nav{
		margin:0 auto;
		width: 25vw;
		height: 25vw;
		background: orange;
		border-radius: 10%;
		display: flex;
		justify-content: center;
		align-items: center;
		// i{
		// 	font-size:100rpx;
		// 	color:#fff;
		// }
		.icon_pic{
			width:20vw;
		}
	}
	.icon_text{
		margin:0 auto;
		font-size: 30rpx;
		line-height: 80rpx;
		color: #333;
		font-family: "Poppins Light";
	}
}
.attr_wrap{
	width:92vw;
	margin:0 auto;
	overflow: hidden;
	margin-top:20rpx;
	.attr_one{
		width:47.5%;
		height:43vw;
		margin-bottom:40rpx;
		text-align: center;
		line-height: 43vw;
		font-size:46rpx;
		color:#fff;
	    float:left;
		background:url('@/static/images/1.jpg') 0 0 /cover no-repeat;
		letter-spacing: 5rpx;
		border-radius: 10rpx;
		&:nth-of-type(odd){
			margin-right:5%;
		}
	}
}
</style>
