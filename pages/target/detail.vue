<template>
	<view class="content-box">
		<view class="mask-box">
			<view class="charts-box">
				<qiun-data-charts type="column" :opts="opts" :chartData="chartData" />
			</view>
			<uni-section title="保费王" class="result-box" type="line">
				<view class="table-box">
					<view class="title">
						<ul>
							<li>区域</li>
							<li>姓名</li>
							<li>保费</li>
						</ul>
					</view>
					<view class="text">
						<div>大一区：</div>
						<div>姓名</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>大二区：</div>
						<div>李四光</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>石一区：</div>
						<div>王晓峰</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>大一区：</div>
						<div>姓名</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>大二区：</div>
						<div>李四光</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>石一区：</div>
						<div>王晓峰</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>大一区：</div>
						<div>姓名</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>大二区：</div>
						<div>李四光</div>
						<div>984654</div>
					</view>
					<view class="text">
						<div>石一区：</div>
						<div>王晓峰</div>
						<div>984654</div>
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
			}
		},
		onShow() {
			this.opts = {
				color: ["#FB243A", "#24F8F2", "#3EAEFF"],
				padding: [15, 15, 0, 5],
				legend: {
					position: 'top',
					fontColor: "#FFFFFF",
					float: 'right',
					margin: 10,
				},
				xAxis: {
					fontColor: "#FFFFFF",
					disableGrid: true,
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
						width: 6,
						activeBgColor: "#000000",
						activeBgOpacity: 0.08,
						seriesGap: 0.5
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
						categories: ['大一', '大二', '石一', '石二', '江北', '沙区', '金阳', '万达', '奥体', '嘉州'],
						series: [{
								name: '总保费',
								textColor: '#FFFFFF',
								textSize: 10,
								data: [35, 2, 31, 33, 13, 34, 33, 13, 34, 79]
							},
							{
								name: '总件数',
								textColor: '#FFFFFF',
								textSize: 10,
								data: [35, 3, 31, 3, 13, 124, 3, 13, 34, 58]
							},
							{
								name: '总旅游名额',
								textColor: '#FFFFFF',
								textSize: 10,
								data: [35, 36, 61, 12, 13, 39, 13, 0, 84, 38]
							}
						]
					};
					this.chartData = JSON.parse(JSON.stringify(res));
				}, 500);
			}
		},
	}
</script>

<style lang="scss" scoped>
	.content-box {
		background: url('@/static/target/detail.png') no-repeat;
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

			.table-box {
				padding: 16px 22px;
				box-sizing: border-box;
				background: rgba(255, 255, 255, 0.8);
				border-radius: 10px;

				.title {
					ul {
						padding: 0px;
						width: 100%;
						display: flex;
						margin-bottom: 16px;
					}

					ul li {
						font-family: PingFang SC;
						font-size: 14px;
						list-style: none;
						flex: 1;

					}
				}

				.text {
					display: flex;

					div {
						flex: 1;
						height: 42px;
						line-height: 42px;
						font-size: 14px;
						border-top: 1px solid rgb(211, 211, 211);
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
