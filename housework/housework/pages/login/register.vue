<template>
	<view class="container" :style="{background: 'url('+imageURL+')'}">
		<view class="back-btn yticon icon-zuojiantou-up" @click="login">
		</view>
		<!-- 设置白色背景防止软键盘把下部绝对定位元素顶上来盖住输入框等 -->
		<view class="wrapper">
			<view class="left-top-sign">LOGIN</view>
			<view class="welcome">
				注册
			</view>
			<view class="input-content">
				<view class="input-item">
					<text class="tit">手机号码</text>
					<input type="number" :value="mobile" placeholder="请输入手机号码" maxlength="11" data-key="mobile" @input="inputChange" />
				</view>
				<view class="input-item">
					<text class="tit">密码</text>
					<input type="mobile" value="" placeholder="8-18位不含特殊字符的数字、字母组合" placeholder-class="input-empty" maxlength="20"
					 password data-key="password" @input="inputChange" />
				</view>
				<view class="input-item">
					<text class="tit">确定密码</text>
					<input type="mobile" value="" placeholder="8-18位不含特殊字符的数字、字母组合" placeholder-class="input-empty" maxlength="20"
					 password data-key="repassword" @input="inputChange" />
				</view>
				<view class="input-item">
					<text class="tit">验证码</text>
					<view class="input-btn"><input class="item-input"  type="mobile" :value="veriCode" placeholder="请输入验证码" maxlength="4"
					data-key="veriCode" @input="inputChange" /><button class="item-btn" type="primary" @click="getVeriCode">{{va.title}}</button></view>
				</view>
			</view>
			<button class="confirm-btn" @click="toRegister">注册</button>
		</view>
	</view>
</template>

<script>
	import {
		registerurl,
		getVeriCode,
		myrequest
	} from '../../api.js';

	export default {
		data() {
			return {
				mobile: '',
				password: '',
				veriCode: '',
				res: '',
				va:{
					title: "获取验证码",
					click: true,
				},
				imageURL: 'http://127.0.0.1:8080/housework/beijing/login.png'
			}
		},
		onLoad() {

		},
		methods: {
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
				if (key == 'password') {
					if (this['mobile'].length != 11) {
						uni.showToast({
							title: '电话长度不等于11位,请重新输入',
							icon: 'none'
						})
					}
				} else if (key == 'repassword') {
					if (this['password'].length < 8) {
						uni.showToast({
							title: '密码的长度要大于八位',
							icon: 'none'
						})
					}
				}
			},
			async getVeriCode(){
				const sendData = {
					name: this.mobile
				};
				console.log("获取验证码");
				const result = await myrequest(getVeriCode, 'POST', sendData);
				console.log(result);
			},
			async toRegister() {
				if (this['repassword'] != this['password']) {
					uni.showToast({
						title: '两次输入密码不一致',
						icon: 'none'
					})
					return 0;
				}
				const {
					mobile,
					password
				} = this;

				const sendData = {
					name: mobile,
					password,
					veriCode: this.veriCode
				};
				const result = await myrequest(registerurl, 'POST', sendData);
				if (result['respCode'] == 0) {
					uni.showToast({
						title: '注册成功，跳转到登录页面',
						icon: 'none',
					})
					uni.navigateBack();
				}
			},
			login(){
				uni.navigateTo({
					url: './login',
				});
			}
		},
		
	}
</script>

<style lang='scss'>
	page {
		background: #fff;
	}
	.input-btn{
		width: 100%;
	}
	.item-btn{
		display: inline-block;
		width: 60px;
		height: 20px;
		font-size: 8px;
		text-align:center;
		line-height: 20px;
		margin: 0;
		margin-left: 7px;
	}
	.container {
		padding-top: 115px;
		position: relative;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		background: #fff;
	}

	.wrapper {
		position: relative;
		z-index: 90;
		padding-bottom: 40upx;
	}

	.back-btn {
		position: absolute;
		left: 40upx;
		z-index: 9999;
		padding-top: var(--status-bar-height);
		top: 40upx;
		font-size: 40upx;
		color: $font-color-dark;
	}

	.left-top-sign {
		font-size: 120upx;
		color: $page-color-base;
		position: relative;
		left: -16upx;
	}

	.right-top-sign {
		position: absolute;
		top: 80upx;
		right: -30upx;
		z-index: 95;

		&:before,
		&:after {
			display: block;
			content: "";
			width: 400upx;
			height: 80upx;
			background: #b4f3e2;
		}

		&:before {
			transform: rotate(50deg);
			border-radius: 0 50px 0 0;
		}

		&:after {
			position: absolute;
			right: -198upx;
			top: 0;
			transform: rotate(-50deg);
			border-radius: 50px 0 0 0;
			/* background: pink; */
		}
	}

	.left-bottom-sign {
		position: absolute;
		left: -270upx;
		bottom: -320upx;
		border: 100upx solid #d0d1fd;
		border-radius: 50%;
		padding: 180upx;
	}

	.welcome {
		position: relative;
		left: 50upx;
		top: -90upx;
		font-size: 46upx;
		color: #555;
		text-shadow: 1px 0px 1px rgba(0, 0, 0, .3);
	}

	.input-content {
		padding: 0 60upx;
	}
	
	.tt{
		display: inline-block;
		width: 60%;
	}

	.input-item {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: center;
		padding: 0 30upx;
		background: $page-color-light;
		height: 120upx;
		border-radius: 4px;
		margin-bottom: 50upx;

		&:last-child {
			margin-bottom: 0;
		}

		.tit {
			height: 50upx;
			line-height: 56upx;
			font-size: $font-sm+2upx;
			color: $font-color-base;
		}
		
		input {
			height: 60upx;
			font-size: $font-base + 2upx;
			color: $font-color-dark;
			width: 100%;
		}
		
		.item-input{
			display: inline-block;
			width: 72%;
		}
		
		.form-cmir{
			display: inline-bolck;
			border: 5px;
			border-color: #4399FC;
			font-size: 10px;
		}
	}

	.confirm-btn {
		width: 630upx;
		height: 76upx;
		line-height: 76upx;
		border-radius: 50px;
		margin-top: 70upx;
		background: $uni-color-primary;
		color: #fff;
		font-size: $font-lg;

		&:after {
			border-radius: 100px;
		}
	}

	.forget-section {
		font-size: $font-sm+2upx;
		color: $font-color-spec;
		text-align: center;
		margin-top: 40upx;
	}

	.register-section {
		position: absolute;
		left: 0;
		bottom: 50upx;
		width: 100%;
		font-size: $font-sm+2upx;
		color: $font-color-base;
		text-align: center;

		text {
			color: $font-color-spec;
			margin-left: 10upx;
		}
	}
</style>
