<template>
	<view class="register">
		<view class="user-logo">
			<image class="logo-image" src="../../static/login.png" mode="aspectFit"></image>
		</view>
		<view class="user-input">
			<div :class="['input-group',{'input-group-tips':tips.email}]">
				<div class="input-text">
					<input type="text" v-model="email" value="" placeholder="邮箱" />
				</div>
				<div class="tips">{{tips.email}}</div>
			</div>
			<div :class="['input-group',{'input-group-tips':tips.nickname}]">
				<div class="input-text">
					<input type="text" v-model="nickname" value="" placeholder="昵称" />
				</div>
				<div class="tips">{{tips.nickname}}</div>
			</div>
			<div :class="['input-group',{'input-group-tips':tips.password}]">
				<div class="input-text">
					<input type="password" v-model="password" value="" placeholder="密码" />
				</div>
				<div class="tips">{{tips.password}}</div>
			</div>
		</view>
		<view class="user-service">
			<label>
				<checkbox class="server-select" value="" />我同意<text class="serviece-text">《用户服务协议》</text>
			</label>
		</view>
		<button class="user-btn" @click="register">注册</button>
		<cu-custom />
	</view>
</template>
<script>
	export default {
		data() {
			return {
				email: "",
				password: "",
				nickname: "",
				tips: {
					email: "",
					password: "",
					nickname: "",
				}
			};
		},
		watch: {
			email() {
				this.emailMatch();
				console.log(this.email)
				return this.email;
			},
			password() {
				this.passwordMatch();
				return this.password;
			},
			nickname() {
				this.nicknameMatch();
				return this.nickname;
			},
		},
		methods: {
			emailMatch() {
				let emailReg = /^\w+@[a-z0-9]+\.[a-z]+$/i;
				if (emailReg.test(this.email)) {
					this.tips.email = '';
				} else {
					this.tips.email = '邮箱格式不正确';
				};
			},
			nicknameMatch() {
				let reg = /^[a-zA-Z\u4e00-\u9fa5]+$/;
				if (reg.test(this.nickname)) {
					this.tips.nickname = "";
					return true;
				} else {
					this.tips.nickname = "昵称只能是汉字或者做字母";
					return false;
				};
			},
			passwordMatch() {
				let reg = /^(?![\d]+$)(?![a-zA-Z]+$)(?![.!#$%^&*]+$)[\da-zA-Z!.#$%^&*]{6,20}$/;
				if (reg.test(this.password)) {
					this.tips.password = "";
					return true;
				} else {
					this.tips.password = "密码必须包含字母数字符号执行少两种";
					return false;
				};
			},
			register() {
				console.log(this.email, this.password, this.nickname)
			},
		},

	}
</script>

<style lang="less">
	.register {
		.user-logo {
			margin: 10upx auto 20upx;
			width: 50%;
			height: auto;

			.logo-image {
				display: inline-block;
				width: 100%;
				height: 200upx;
			}
		}

		.user-input,
		.user-service,
		.user-btn {
			margin: 0 auto;
			width: 70%;
		}

		.user-input {
			.input-group {
				margin: 8upx auto 0;

				.input-text {
					box-shadow: 0 0 8upx 4upx #AAAAAA;
					padding: 10upx 20upx;
					border-radius: 50upx;
					border: 2upx solid #AAAAAA;

					&:focus-within {
						border-color: #41b881;
						box-shadow: 0 0 8upx 4upx #41b881;
					}
				}

				.tips {
					padding: 12upx 0 6upx;
					line-height: 1;
					height: 0;
					color: #ED1C24;
					font-size: 28upx;
					text-indent: 16upx;
					transition: all .2s;
				}
			}

			.input-group-tips {
				.input-text {
					box-shadow: 0 0 8upx 4upx #ED1C24;
					border-color: #ED1C24;
				}

				.tips {
					height: 28upx;
				}
			}
		}

		.user-service {
			margin: 32upx auto;
			font-size: 30upx;

			.uni-checkbox .uni-checkbox-inputt {
				width: 32upx;
			}

			.serviece-text {
				color: #41b881;
			}
		}

		.user-btn {
			margin: 80upx auto 0;
			width: 68%;
			background-color: #41b881;
			color: #eee;
			font-size: 32upx;
		}
	}
</style>
