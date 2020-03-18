<template>
	<view class="chaplist" v-if="chaplist.article_title">
		<view class="chaplist-intro">
			<view class="intro-wrap padding-x">
				<image class="intro-image" :src="chaplist.cover_image" mode="widthFix"></image>
				<view class="intro-text">
					<view class="intro-title">{{chaplist.article_title}}</view>
					<view>作者: {{chaplist.author}}</view>
					<view>{{chaplist.article_status}}</view>
				</view>
			</view>
			<view class="article-intro">
				<text class="article-title">文章简介:</text> {{chaplist.article_intro}}
			</view>
		</view>
		<view class="intro-list-title padding-x">
			相关推荐
		</view>
		<view class="chaplist-recoment padding-x">
			<text @click="pageRouter(item.link)" class="list-item"  v-for="(item,index) in chaplist.recomment" :key="index">
				{{item.title}}
			</text>
		</view>
		<view class="intro-list-title padding-x">
			章节列表
		</view>
		<view class="chaplist-list padding-x">
			<view @click="pageRouter(item.link)" class="list-item"  v-for="(item,index) in chaplist.article_list" :key="index">
				{{item.title}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				chaplist:{},
				link:""
			};
		},
		onLoad(options) {
			console.log(options.id)
			this.getChaplist(options.id);
		},
		methods:{
			pageRouter(item){
				console.log(this.link+item)
				uni.navigateTo({
				    url: './detail?id='+this.link+item,
				    animationType: 'pop-in',
				    animationDuration: 200
				});
			},
			getChaplist(id){
				this.link = id;
				uni.request({
					url: 'http://localhost:9926/qqxs/chaplist?id='+id, 
					method: "GET",
					success: (res) => {
						this.chaplist = res.data.articleList;
						console.log(this.chaplist)
					}
				})
			}
		}
	}
</script>

<style lang="less">
.chaplist{
	.padding-x{
		padding:0 16upx;
	}
	.chaplist-intro{
		.intro-wrap{
			margin-bottom:10upx;
			display: flex;
			justify-content: space-between;
			background-image:linear-gradient(to bottom, #41b881 20%, #41b881 65%, transparent 65%);
			.intro-image{
				width: 45%;
			}
			.intro-text{
				box-sizing: border-box;
				width: 52%;
				line-height: 1.5;
				font-size: 30upx;
				color: #fff;
				.intro-title{
					font-weight: bold;
					font-size: 34upx;
				}
			}
		}
	}
	.article-intro{
		line-height: 1.4;
		padding: 10upx;
		font-size: 32upx;
		text-align: justify;
		color: #2C405A;
		.article-title{
			font-weight: bold;
		}
	}
	.chaplist-recoment{
		.list-item{
			display: inline-block;
			margin: 8upx;
			padding: 8upx 20upx;
			background-color: #eee;
			font-size: 30upx;
			border-radius: 10upx;
		}
	}
	.intro-list-title{
		margin: 8upx 0;
		line-height: 2;
		font-size: 32upx;
		font-weight: bold;
		border-bottom: 4upx solid #eee;
	}
	.chaplist-list{
		.list-item{
			line-height: 1.5;
			font-size: 32upx;
			
		}
	}
}
</style>
