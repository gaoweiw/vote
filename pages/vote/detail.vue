<template>
	<view class="content-box">
		<view class="mask-box">
			<view class="charts-box">
				<qiun-data-charts type="column" :opts="opts" :chartData="chartData" />
			</view>
			<uni-section title="投票结果" class="result-box" type="line">
				<view class="item-box" v-for="item in resultArray">
					<view class="title">
						{{item.cityName}}：
					</view>
					<view class="bar">
						<view class="bar-color" :style="{'width':item.percent + '%','background':item.background}">
						</view>
						<view class="bar-text">
							{{item.number}}票
						</view>
					</view>
				</view>
			</uni-section>
		</view>
	</view>
</template>

<script>
	import baseUrl from '@/util/baseUrl';
	export default {
		data() {
			return {
				data: {},
				chartData: {},
				opts: {},
				resultArray: [], //投票结果
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
					tooltip: {
						showBox: false
					},
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
				uni.request({
					url: `${baseUrl}/auth/vote/list`,
					success: (response) => {
						this.data = response.data.rows;
						console.log(response);


						let desc = function(a, b) {
							return b - a
						};
						let asc = function(a, b) {
							return a - b
						};
						let numArray = [];
						this.data.forEach(item => {
							numArray.push(item.number)
						})
						let maxNumber = numArray.sort(desc)[0];
						let showMax = 0; //线条最大值，用于显示票数占比
						if (maxNumber > 10) {
							showMax = maxNumber + parseInt(maxNumber / 4)
						} else {
							showMax = maxNumber;
						}

						//投票结果
						this.resultArray = [];
						this.data.forEach(item => {
							let obj = {
								cityName: item.cityName,
								background: item.cityName == "北京" ? '#FF1E1E' : item.cityName ==
									"内蒙" ? '#24F8F2' : item.cityName == "新疆" ? '#3EAEFF' : '',
								number: item.number,
								percent: item.number > 0 ? parseFloat(item.number / showMax) *
									100 : 0
							}
							this.resultArray.push(obj);
						})


						let res = {
							categories: ['北京', '内蒙', '新疆'],
							series: [{
								name: '',
								textColor: '#FFFFFF',
								format: 'voteFormat',
								data: [{
										color: '#FF1E1E',
										value: this.data.filter(res => res.cityName == '北京')[0]
											.number,

									},
									{
										color: '#24F8F2',
										value: this.data.filter(res => res.cityName == '内蒙')[0]
											.number,
									},
									{
										color: '#3EAEFF',
										value: this.data.filter(res => res.cityName == '新疆')[0]
											.number,
									}
								]
							}]
						};
						this.chartData = JSON.parse(JSON.stringify(res));

					}
				});
			}
		},
	}
</script>

<style lang="scss" scoped>
	.content-box {
		height: 100vh;
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
