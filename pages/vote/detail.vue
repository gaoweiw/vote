<template>
	<view class="content-box">
		<view class="mask-box">
			<view class="charts-box">
				<qiun-data-charts type="column" :opts="opts" :chartData="chartData" />
			</view>
			<uni-section title="投票结果" class="result-box" type="line">
				<view class="item-box">
					<view class="title">
						北京：
					</view>
					<view class="bar">
						<view class="bar-color" :style="{'width':bjWidth,'background':'#FF1E1E'}"></view>
						<view class="bar-text">
							300票
						</view>
					</view>
				</view>
				<view class="item-box">
					<view class="title">
						内蒙：
					</view>
					<view class="bar">
						<view class="bar-color" :style="{'width':nmWidth,'background':'#24F8F2'}"></view>
						<view class="bar-text">
							300票
						</view>
					</view>
				</view>
				<view class="item-box">
					<view class="title">
						新疆：
					</view>
					<view class="bar">
						<view class="bar-color" :style="{'width':xjWidth,'background':'#3EAEFF'}"></view>
						<view class="bar-text">
							300票
						</view>
					</view>
				</view>
			</uni-section>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				chartData: {},
				opts: {},
				bjWidth: "",
				nmWidth: "",
				xjWidth: "",

			}
		},
		onShow() {
			this.opts = {
				color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
					"#ea7ccc"
				],
				padding: [15, 15, 0, 5],
				legend: {
					position: 'top',
					fontColor: "#FFFFFF",
					float: 'right',
					margin: 10,
				},
				xAxis: {
					fontColor: "#FFFFFF",
					disableGrid: true
				},
				yAxis: {
					disableGrid: true, //不绘制横向网格
					data: [{
						fontColor: "#FFFFFF",
						min: 0
					}]
				},
				extra: {
					column: {
						width: 20,
						activeBgColor: "#000000",
						activeBgOpacity: 0.08
					}
				}
			}
		},
		onReady() {
			this.getServerData();
		},
		methods: {
			getServerData() {
				//模拟从服务器获取数据时的延时
				setTimeout(() => {
					//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接

					this.bjWidth = '20%'
					let res = {
						categories: ['北京', '内蒙', '新疆'],
						series: [{
							name: '票数',
							textColor: '#FFFFFF',
							data: [{

									color: '#FB243A',
									value: 45,

								},
								{
									color: '#24F8F2',
									value: 342
								},
								{
									color: '#3EAEFF',
									value: 35
								}
							]
						}]
					};
					this.chartData = JSON.parse(JSON.stringify(res));
				}, 500);
			}
		},
	}
</script>

<style lang="scss" scoped>
	.content-box {
		height: calc(100vh - 44px);
		background: url('@/static/vote/result-bgk.png') no-repeat;
		background-size: cover;

		.mask-box {
			width: 100%;
			height: 100%;
			background: rgba(0, 0, 0, 0.4);
		}

		.charts-box {
			padding: 14px;
			box-sizing: border-box;
			min-height: 278px;
		}

		.result-box {
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
	}

	:deep(.uni-section__content-title) {
		color: #FFFFFF !important;
		font-size: 16px;
	}

	:deep(.uni-section .uni-section-header__decoration) {
		background-color: #FF1E1E !important;
	}
</style>
