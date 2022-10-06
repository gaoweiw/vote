<template>
	<view class="content-box">
		<view class="mask-box">
			<uni-section title="录入信息" class="result-box" type="line">

				<uni-forms class="form-box" :modelValue="formData" :label-width="120">
					<uni-forms-item name="code" label="营 业 区">
						<uni-data-select @change="rangeChange" v-model="formData.code" placeholder="请筛选"
							:localdata="range">
						</uni-data-select>
					</uni-forms-item>
					<uni-forms-item label="姓　 名" name="name">
						<input type="text" v-model.trim="formData.userName" placeholder="请输入姓名" />
					</uni-forms-item>
					<uni-forms-item label="1108保费" name="premium">
						<input type="number" v-model.trim="formData.premium" placeholder="请输入保费" />
					</uni-forms-item>
					<uni-forms-item label="1108件数" name="number">
						<input type="number" v-model.trim="formData.number" placeholder="请输入件数" />
					</uni-forms-item>
					<uni-forms-item label="1108旅游名额" name="quota">
						<input type="number" v-model.trim="formData.quota" placeholder="请输入旅游名额" />
					</uni-forms-item>

				</uni-forms>
			</uni-section>
			<view type="default" class="button" @click="submitClick">录入完成</view>
		</view>
		<view class="target-result" @click="jumpResult">
			录入结果
		</view>
		<uni-popup ref="popup" type="dialog">
			<view class="pop-wrapper">
				<view class="pop-success">
					录入完成
				</view>
				<view class="pop-close" @click="close">
					确定
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import baseUrl from '@/util/baseUrl';
	export default {
		data() {
			return {
				formData: {
					code: "",
					name: "",
					userName: "",
					premium: "",
					number: "",
					quota: "",
				},
				value: 1,
				textArray: ['大一', '大二', '石一', '石二', '江北', '沙区', '金阳', '万达', '奥体', '嘉州'],
				range: [],
				targetNum: 0,
				isRun: true,
			};
		},
		onLoad() {
			var a = document.getElementsByClassName('uni-page-head-hd')[0]
			a.style.display = 'none';


		},
		mounted() {
			this.textArray.forEach((item, index) => {
				this.range.push({
					value: index + 1,
					text: item
				})
			})
		},
		methods: {
			submitClick() {
				//每个人只能录入两次，结束时间10月15日
				let nowTime = Date.now();
				let endTime = (new Date('2022/10/15 23:59:59')).valueOf();

				if (nowTime > endTime) {
					uni.showToast({
						icon: 'error',
						title: '录入已经结束 ',
					})
					return;
				}

				this.targetNum = window.localStorage.getItem('targetNmuber');
				if (this.targetNum >= 2) {
					uni.showToast({
						icon: 'error',
						title: '每人只能录入两次 ',
					})
				} else {
					this.targetRequest();
				}
			},
			targetRequest() {


				if (!this.formData.code) {
					uni.showToast({
						icon: 'error',
						title: '请选择营业区',
					})
					return;
				} else if (!this.formData.userName) {
					uni.showToast({
						icon: 'error',
						title: '请填写姓名',
					})
					return;
				} else if (!this.formData.premium) {
					uni.showToast({
						icon: 'error',
						title: '请填写保费',
					})
					return;
				} else if (this.formData.premium < 1000) {
					uni.showToast({
						icon: 'error',
						title: '保费必须大于等于1000',
					})
					return;
				} else if (!this.formData.number) {
					uni.showToast({
						icon: 'error',
						title: '请填写件数',
					})
					return;
				} else if (!this.formData.quota) {
					uni.showToast({
						icon: 'error',
						title: '请填写名额',
					})
					return;
				}
				if (!this.isRun) return;
				this.isRun = false;
				uni.request({
					url: `${baseUrl}/auth/target/save`,
					method: "POST",
					data: this.formData,
					success: (res) => {
						this.$refs.popup.open();
						window.localStorage.setItem('targetNmuber', Number(this.targetNum) + 1); //录入次数

						this.formData = {
							code: "",
							name: "",
							userName: "",
							premium: "",
							number: "",
							quota: "",
						}
						this.isRun = true;
					}
				});
			},
			close() {
				this.$refs.popup.close()
			},
			rangeChange(e) {
				console.log(e);
				if (e) {
					this.formData.name = this.textArray[e - 1];
				} else {
					this.formData.name = '';
				}
			},
			jumpResult() {
				uni.navigateTo({
					url: '/pages/target/detail',
					animationType: 'slide-in-right',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}

	}
</script>

<style lang="scss" scoped>
	.content-box {
		position: relative;
		height: 100vh;
		background: url("@/static/target/bgk.png") no-repeat;
		background-size: cover;

		.mask-box {
			width: 100%;
			height: 100%;
			background: rgba(0, 0, 0, 0.4);
		}

		.form-box {
			margin-top: 20px;

			:deep(.uni-forms-item) {
				border: 1px solid #FFFFFF;
				border-radius: 10px;
				height: 48px;
				line-height: 48px;
			}

			:deep(.uni-forms-item__label) {
				height: 48px;
				line-height: 48px;
				color: #FFFFFF;
				padding-left: 17px;
			}

			:deep(.uni-select__input-placeholder) {
				color: #FFFFFF;
				font-size: 14px;
			}

			:deep(.uni-select__input-text) {
				color: #FFFFFF;
				letter-spacing: 2px;
			}

			:deep(.uni-select) {
				border: none;
				height: 48px;
				line-height: 48px;
			}

			:deep(.uni-easyinput__content) {
				background-color: none;
			}

			:deep(.uni-input-wrapper) {
				height: 48px;
				line-height: 48px;
				color: #FFFFFF;
			}

			:deep(.uni-input-placeholder) {
				color: #FFFFFF;
			}

			uni-input {
				height: auto;
			}
		}

		.pop-wrapper {
			width: calc(100vw - 56px);
			margin: 14px;
			box-sizing: border-box;
			background: #FFFFFF;
			border-radius: 10px;
			padding: 14px;
			text-align: center;

			.pop-success {
				font-size: 16px;
				line-height: 80px;
				margin-bottom: 20px;
			}

			.pop-close {
				color: #FFFFFF;
				border-radius: 50px;
				width: 100px;
				text-align: center;
				line-height: 32px;
				margin: 0 auto;
				background-color: #007aff;
			}
		}

		.result-box {
			position: relative;
			z-index: 10;
			padding: 14px;
			box-sizing: border-box;
			background: none;

			.item-box:first-child {
				margin-top: 20px;
			}

			.item-box {
				display: flex;
				padding: 0 0 0 14px;
				color: #FFFFFF;
				font-size: 14px;
				margin-top: 32px;


				.title {
					flex: 0 0 50px;
				}

				.bar {
					flex: 0 0 80%;
					height: 6px;
					position: relative;
					border-radius: 8px;
					background-color: #FFFFFF;
					margin-top: 8px;

					.bar-color {
						border-radius: 8px;
						width: 40%;
						height: 6px;
					}

					.bar-text {
						position: absolute;
						top: -20px;
						font-size: 10px;
						right: 0;
						color: #FFFFFF;
					}
				}
			}
		}

		.target-result {
			position: absolute;
			z-index: 101;
			top: 22px;
			right: 0;
			width: 64px;
			height: 21px;
			line-height: 21px;
			background: rgb(255, 30, 30);
			color: #FFFFFF;
			text-align: center;
			font-size: 11px;
			border-radius: 20px 0px 0px 20px;
		}

		.button {
			background: #FF1E1E;
			color: #FFFFFF;
			width: 153px;
			height: 42px;
			text-align: center;
			line-height: 42px;
			border-radius: 50px;
			margin: 61px auto 0 auto;
		}
	}

	:deep(.uni-section__content-title) {
		color: #FFFFFF !important;
		font-size: 16px;
	}

	:deep(.uni-section .uni-section-header__decoration) {
		background-color: #FF1E1E !important;
	}
</style>
