<template>
	<view class="index">
		<view class="banner">
			<button class="banner-btn priv " @click="priv">
				P
			</button> <button class="banner-btn next" @click="next">
				N
			</button>
			<!-- :style="{'transition-delay': index *50+'ms'}" -->
			<view :class="bannerClassName[index]"  v-for="(item,index) in homeData" :key="index">
				<image @click="pageRouter(item.link)" class="item-image" :src="item.cover_image" mode="scaleToFill"></image>
			</view>
		</view>
		<view class="recomment-title">
			热门推荐
		</view>
		<view class="recoment">
			<view class="list-item" v-for="(item,index) in hotData" :key="index">
				<image @click="pageRouter(item.link)" class="item-image" :src="item.cover_image" mode="scaleToFill"></image>
				<text class="item-title">{{item.title}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				homeData: {},
				hotData:{},
				bannerClassName: ['item-left-leave', 'item-active-enter', 'item-right-leave', 'item-right-read']
			};
		},
		onLoad() {
			this.getHomeData()
			this.getHotData()
		},
		methods: {
			pageRouter(item){
				console.log(item)
				uni.navigateTo({
				    url: './chaplist?id='+item,
				    animationType: 'pop-in',
				    animationDuration: 200
				});
			},
			priv() {
				this.bannerClassName.push(this.bannerClassName[0])
				this.bannerClassName.shift()
			},
			next() {
				this.bannerClassName.unshift(this.bannerClassName[this.bannerClassName.length - 1])
				this.bannerClassName.pop()
			},
			getHotData(){
				uni.request({
					url: 'http://localhost:9926/qqxs/category', //仅为示例，并非真实接口地址。
					method: "GET",
					success: (res) => {
						this.hotData = res.data.result.list;
					}
				})
			},
			getHomeData() {
				uni.request({
					url: 'http://localhost:9926/qqxs/home', //仅为示例，并非真实接口地址。
					method: "GET",
					success: (res) => {
						this.homeData = res.data.result.banner;
						this.setBannerClassName()
					}
				})
			},
			setBannerClassName(){
				for(let i=0;i<this.homeData.length-4;i++){
					this.bannerClassName.push('banner-item')
				}
			}
		}
	}
</script>

<style lang="less">
	.index {
		background-color: #eee;
		.banner {
			position: relative;
			margin-bottom: 5px;
			width: 100%;
			height: 220px;
			transform-style: preserve-3d;
			perspective: 800px;
		
			.banner-item,
			.item-left-leave,
			.item-active-enter,
			.item-right-leave,
			.item-right-read {
				position: absolute;
				top: 50%;
				left: 50%;
				transform: translate(-50%, -50%);
				opacity: 0;
				visibility: hidden;
				width: 30%;
				height: 180px;
				background-color: #ffffff;
				transition: .3s;
		
				.item-image {
					width: 100%;
					height: 100%;
				}
			}
		
			.item-left-leave,
			.item-active-enter,
			.item-right-leave,
			.item-right-read {
				opacity: 1;
				visibility: visible;
			}
			.item-left-leave,
			.item-right-leave,
			.item-right-read{
				filter: blur(3px);
			}
			.item-left-leave {
				left: 0;
				transform-origin: left;
				transform: translateY(-50%) rotateY(60deg) scale(1.2);
			}
		
			.item-active-enter {
				left: 50%;
				transform: translate(-80%, -50%) scale(1.2);
			}
		
			.item-right-leave {
				right: 0;
				transform-origin: right;
				transform: translate(7%, -50%) rotateY(-55deg) scale(1.2);
			}
		
			.item-right-read {
				right: 0;
				transform-origin: left;
				transform: translate(112%, -50%) rotateY(10deg) scale(1.2);
			}
		
			.banner-btn {
				position: absolute;
				top: 50%;
				transform: translateY(-50%);
				line-height: 90upx;
				background-color: rgba(255, 255, 255, 0.3);
				padding: 0 15upx;
				font-weight: bold;
			}
		
			.priv {
				left: 0;
			}
		
			.next {
				right: 0;
			}
		}
		.recoment{
			margin: 10upx;
			display: flex;
			justify-content: center;
			flex-wrap: wrap;
			background-color: #fff;
			.list-item{
				margin: 6upx;
				width: 31%;
				.item-image{
					width: 100%;
					height: 360upx;
				}
				.item-title{
					font-size: 28upx;
				}
			}
		}
	}
</style>
