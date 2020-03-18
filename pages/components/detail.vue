<template>
	<view class="detail">
		<view class="article" v-for="(item,index) in article" :key="index">
			<text class="article-title">{{item.title}}</text>
			<view class="content" v-for="(ite,index) in item.content" :key="index">
				{{ite}}
			</view>
		</view>
		<button class="article-more" @click="getArticle(article[article.length-1].article_next)">下一章</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				article:[]
			};
		},
		onLoad(opts) {
			this.getArticle(opts.id)
		},
		methods:{
			getArticle(id){
				uni.request({
					url: 'http://localhost:9926/qqxs/detail?id='+id, //仅为示例，并非真实接口地址。
					method: "GET",
					success: (res) => {
						this.article.push(res.data.result);
					}
				})
			}
		}
	}
</script>

<style lang="less">
.detail{
	line-height: 1.5;
	padding: 0 20upx;
	text-align: justify;
	color: #2C405A;
	.article-title{
		line-height: 3;
		font-size: 34upx;
		font-weight: bold;
	}
	.content{
		margin-bottom: 30upx;
		line-height: 2;
		font-size: 34upx;
		text-indent: 2em;
	}
	.article-more{
		display: block;
		margin: 10upx 0;
		padding: 8upx;
		width: 100%;
		text-align: center;
		font-size:32upx;
		border: none;
	}
}
</style>
